---
layout: post
title: "How I secure my VPS"
excerpt: "Configurations for UFW, Fail2ban, SSH and other security tips."
date: 2022-08-20
tags: [self-hosting, server, security]
comments: true
---

As I said in my [Spring 2022 overview](https://www.fuzzygrim.com/seasons/spring-2022), I've rented a VPS to host [Umami](https://github.com/mikecao/umami) and therefore I decided to take notes on the steps I took to ensure a better security for my system. 

This post is essentially for self-reference, but you may find it useful as well. However, take into account that I am running Debian, so some commands might not work if you are using another distribution.

## Update your system
The most important step is to have your system up to date, run this: `sudo apt update && sudo apt upgrade`

## Change root password
Another essential step is to change the default root password provided by the VPS provider, this can be done with: `passwd root`

## Create a new user with restricted rights
General tasks should be done as a regular user, so you should create a new user with the following steps:

- Create a new user: `adduser username`

- Install `sudo` with: `apt install sudo`

- Add the user to the `sudo` group: `usermod -aG sudo username`

- You can run `docker` commands without sudo by adding your user to the `docker` group: `usermod -aG docker username`, but be aware of the [security issues](https://docs.docker.com/engine/security/#docker-daemon-attack-surface) of doing so.


## Disable root
Now that you have a user who can use `sudo` to run root level commands when needed, you should disable the root user to avoid future problems `sudo passwd -l root`, this should return the following line: 

```
passwd: password expiry information changed
```

## SSH hardening

### Key-based authentication:
By using SSH-Keys, you can make sure that you are the only one accessing your VPS and authenticate without a password. To start using this authentication method, you will need to generate a key if you don't have one already with `ssh-keygen`, you can check if you already have a key by checking if `/home/username/.ssh/id_rsa` exists.

Then you will need to copy your key to your remote server with the following command:

```
ssh-copy-id username@IP
```

### SSHD_CONFIG

Add or modify the following lines in the `/etc/ssh/sshd_config` file, you can see what each changes do by running `man ssh_config`:

```
PermitRootLogin no
LogLevel VERBOSE
MaxAuthTries 3
MaxSessions 2
PasswordAuthentication no
UsePAM no
AllowAgentForwarding no
AllowTcpForwarding no
X11Forwarding no
Compression no
ClientAliveCountMAx 2
```

Restart ssh to apply the changes with: `sudo systemctl restart ssh`

### SSH Logs

You should also check your ssh logs, this can be read with: `sudo journalctl -t sshd`

## Automatic security updates

I like to use unattended-upgrades to install automatic security updates and automatically reboot when necessary.

Install unattended-upgrades with: `sudo apt-get install unattended-upgrades`

Generate a sample configuration with: `sudo dpkg-reconfigure --priority=low unattended-upgrades`

Now you should have these two lines on `/etc/apt/apt.conf.d/20auto-upgrades`
```
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Upgrade "1";
```

To automatically remove obsolete packages, add the following line to `/etc/apt/apt.conf.d/20auto-upgrades`:
```
APT::Periodic::AutocleanInterval "7";
```

And add to `/etc/apt/apt.conf.d/50unattended-upgrades`:
```
Unattended-Upgrade::Remove-Unused-Dependencies "true";
```

For automatic rebooting the system after updates that requires it, add the following line to `/etc/apt/apt.conf.d/50unattended-upgrades`:
```
Unattended-Upgrade::Automatic-Reboot "true";
Unattended-Upgrade::Automatic-Reboot-Time "02:00";
```

## UFW
Install a firewall to manage all your incoming and outgoing requests, I decided to use UFW which stands for "Uncomplicated Firewall". Run the following commands to get a good starting point:

```
sudo apt install ufw
sudo ufw default allow outgoing comment 'allow all outgoing traffic'
sudo ufw default deny incoming comment 'deny all incoming traffic'
sudo ufw limit in ssh comment 'allow SSH connections in'
sudo ufw enable
sudo ufw status
```

If you run docker containers in the VPS, you will find out that docker ignores your firewall, to fix the docker and UFW [security flaw](https://github.com/chaifeng/ufw-docker#problem) without disabling iptables, append the following code to `/etc/ufw/after.rules`:
```
# BEGIN UFW AND DOCKER
*filter
:ufw-user-forward - [0:0]
:ufw-docker-logging-deny - [0:0]
:DOCKER-USER - [0:0]
-A DOCKER-USER -j ufw-user-forward

-A DOCKER-USER -j RETURN -s 10.0.0.0/8
-A DOCKER-USER -j RETURN -s 172.16.0.0/12
-A DOCKER-USER -j RETURN -s 192.168.0.0/16

-A DOCKER-USER -p udp -m udp --sport 53 --dport 1024:65535 -j RETURN

-A DOCKER-USER -j ufw-docker-logging-deny -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -d 192.168.0.0/16
-A DOCKER-USER -j ufw-docker-logging-deny -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -d 10.0.0.0/8
-A DOCKER-USER -j ufw-docker-logging-deny -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -d 172.16.0.0/12
-A DOCKER-USER -j ufw-docker-logging-deny -p udp -m udp --dport 0:32767 -d 192.168.0.0/16
-A DOCKER-USER -j ufw-docker-logging-deny -p udp -m udp --dport 0:32767 -d 10.0.0.0/8
-A DOCKER-USER -j ufw-docker-logging-deny -p udp -m udp --dport 0:32767 -d 172.16.0.0/12

-A DOCKER-USER -j RETURN

-A ufw-docker-logging-deny -m limit --limit 3/min --limit-burst 10 -j LOG --log-prefix "[UFW DOCKER BLOCK] "
-A ufw-docker-logging-deny -j DROP

COMMIT
# END UFW AND DOCKER
```

Run `sudo systemctl restart ufw` or `sudo ufw reload` to restart UFW. Now the public network can't access any published docker ports, the container and the private network can visit each other normally, and the containers can also access the external network from inside.

To allow public networks to access the services provided by Docker: `ufw route allow proto tcp from any to any port 80`, if you publish a port by using option `-p 8080:80`, you should use the container port `80`, not the host port `8080`.

If there are multiple containers with a service port of `80`, but you only want the external network to access a certain container. For example, if the private address of the container is `172.17.0.2`, use the following command: `ufw route allow proto tcp from any to 172.17.0.2 port 80`. You cand find the `IPAdress` with: 

{% raw %}
```
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' serviceName
```
{% endraw %}

## Fail2ban
This software will prevent brute force login attacks against your server by banning the IP address of the attacker. Start by installing fail2ban with: `sudo apt install fail2ban`, then copy the provided configuration template with: `sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local`

You can configure `/etc/fail2ban/jail.local` to set different rules for each of your services, this is an example configuration for ssh:

```
[sshd]
enabled = true
banaction = ufw
port = ssh
filter = sshd
backend = systemd
logpath = %(sshd_log)s
maxretry = 3
findtime = 5m
bantime  = -1
```
This will ban the IP address of the attacker permanently after the third unsuccessful login attempt in 5 minutes.

Here you have some useful fail2ban commands:
```
sudo fail2ban-client start
sudo fail2ban-client reload
sudo fail2ban-client status
sudo fail2ban-client status sshd
```

## Conclusion
And that would be it! This is how I am currently protecting my server, what do you think? If you have any suggestions, please let me [know](mailto:fuzzygrim@protonmail.com).