---
layout: post
title: "Upgrading Proxmox and Turnkey Debian"
date: 2023-08-18
excerpt: "Keeping devices and software up to date"
tags: [self-hosting, tutorial]
---

Today I decided to upgrade my Proxmox server as the Proxmox team recently released version 8.x, which is based on Debian 12 (Bookworm). This also reminded me that I should upgrade my Turnkey Debian LXCs. I will use this post to document the process and how to fix some bugs I encountered.

## Proxmox

### Upgrading Proxmox 7.x to Proxmox 8.x

As usual, the first step before diving into an upgrade is to make sure you have a backup of your virtual machines and containers. I like to use Proxmox's [built-in backup](https://pve.proxmox.com/wiki/Backup_and_Restore) feature to back up my virtual machines and containers.

Once you have backed up your virtual machines and containers, you can start the upgrade process. You must first ensure that you are running the latest version of Proxmox 7.x with the latest packages. You can do this by running the following commands:

```bash
apt update
apt dist-upgrade
```

Now that you are running the latest version of Proxmox 7.x, you can start the upgrade process to Proxmox 8.x. Since the new version is based on Debian 12, you will need to change the sources.list file to point to the new Debian 12 repositories. You can do this by executing:

```bash
sed -i 's/bullseye/bookworm/g' /etc/apt/sources.list
```

Your `sources.list` file should now look like this:

```bash
deb http://ftp.es.debian.org/debian bookworm main contrib

deb http://ftp.es.debian.org/debian bookworm-updates main contrib

# Proxmox VE pve-no-subscription repository provided by proxmox.com,
# NOT recommended for production use
deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription

# security updates
deb http://security.debian.org bookworm-security main contrib
```

If you have files in the `/etc/apt/sources.list.d/` directory, you will need to change them as well. You can replace `bullseye` with `bookworm` in all the files in that directory by running the following command:

```bash
find /etc/apt/sources.list.d/ -type f -exec sed -i 's/bullseye/bookworm/g' {} \;
```

Now that you have changed the sources.list file, you can run refresh the package index and upgrade your system with:

```bash
apt update
apt dist-upgrade
```

During the upgrade process, you will be asked whether you want to keep the current version of the configuration files or install the new version. You will need to check the differences between the two versions and decide which one you want to keep. As a general rule, you should keep the current version of the configuration files if you have manually made any changes to them. If you have not made any changes to the configuration files, you should install the new version.

Once the upgrade process is complete, you can check for potential issues by running:

```bash
pve7to8
```

If you notice any problems, you must fix them before continuing. If you do not see any problems, you can restart the system with `reboot`.

If everything went well, you should now be running Proxmox 8.x. Now you can purge orphan dependencies by running the following command:

```bash
apt autoremove --purge
```

### Fix stuck on boot

In my case, the upgrade process went fine, but when I rebooted the system, it got stuck at startup with the following line:

```bash
Loading Linux 6.2.16-8-pve ...
```

Fortunately, I have found a solution on [Proxmox forum](https://forum.proxmox.com/threads/hangs-on-screen-loading-linux6-2-13-pve.129355/post-567253), to solve this problem, once your system gets to the GRUB menu, choose the `Advanced options for Proxmox VE` option and select a previous kernel version that boots, after logging in, try to execute the following command:

```bash
apt install pve-kernel-6.2.16-8 # replace 6.2.16-8 with the kernel version that is causing the issue
```

If you get a message saying that the package could not be installed, try reading the error message which should tell you which folder is causing the problem (it should be something in `/var/lib`). Try moving the folder to another location and try installing the package again. Once the package is installed, you can reboot the system and it should boot normally.


## Turnkey Debian

### Upgrading Turnkey Debian 10.x to Turnkey Debian 11.x

Update your system to the latest version of Turnkey Debian 10.x and remove unnecessary packages by running the following commands:

```bash
sudo apt update
sudo apt upgrade
sudo apt --purge autoremove 
```

Download the Turnkey repository key by running the [following command](https://www.turnkeylinux.org/forum/support/sat-20220101-1442/how-upgrade-v16-bullseye-without-tklbam):

```bash
CODENAME=bullseye
key_dir=/usr/share/keyrings
base_url=https://raw.githubusercontent.com/turnkeylinux/common/master/overlays/bootstrap_apt
repos=(main security testing)
for repo in ${repos[@]}; do
    local_path=$key_dir/tkl-$CODENAME-$repo
    keyring=$local_path.gpg
    keyfile=$local_path.asc
     key_url=${base_url}${keyfile}
    wget -O $keyfile $key_url
    gpg --no-default-keyring --keyring $keyring --import $keyfile
    rm $keyfile
done
```

Change the sources.list file to point to the new Turnkey Debian 11.x repositories by running:

```bash
sudo find /etc/apt/sources.list.d/ -type f -exec sed -i 's/buster/bullseye/g' {} \;
```

You will need to change some lines of `/etc/apt/sources.list.d/security.sources.list`, mainly the `bullseye/updates` instances to `bullseye-security`. You can do this by executing the following command:

```bash
sudo sed -i 's/bullseye\/updates/bullseye-security/g' /etc/apt/sources.list.d/security.sources.list
```

It should look like this after the change:

```bash
deb [signed-by=/usr/share/keyrings/tkl-bullseye-security.gpg] http://archive.turnkeylinux.org/debian bullseye-security main

deb http://security.debian.org/ bullseye-security main
deb http://security.debian.org/ bullseye-security contrib
#deb http://security.debian.org/ bullseye-security non-free
```

Now to refresh the package index you can run the following command:

```bash
sudo apt update
```

If everything went well, run the following command to upgrade your existing packages without installing new packages:

```bash
sudo apt upgrade --without-new-pkgs
```

As with the Proxmox upgrade, you will be asked if you want to keep the current version of the configuration files or if you want to install the new version.

Once the minimal upgrade process is complete, run the following command to upgrade your system to the latest version of Turnkey Debian

```bash
sudo apt full-upgrade
```

After the upgrade process is complete, you will need to reboot your system and you should now be running Debian 11.x. You can check your current Debian version by running the following command:

```bash
lsb_release -a
```

Which should output something like this:

```bash
Distributor ID: Debian
Description:    Debian GNU/Linux 11 (bullseye)
Release:        11
Codenme:        bullseye
```

If everything seems to be working fine, you can purge orphan dependencies by running:

```bash
sudo apt --purge autoremove -y
```

### Fix unsafe path transition error

If you are getting the following error when trying to upgrade your system:

```bash
Detected unsafe path transition / -> /var during canonicalization of /var/cache.
dpkg: dependency problems prevent configuration of libpam-systemd:amd64:
 libpam-systemd:amd64 depends on systemd (= 247.3-7+deb11u2); however:
  Package systemd is not configured yet.
```

This [may be caused](https://askubuntu.com/a/1244984) if the root doesn't own `/`, `/var` or `/dev`, if that is the case, you can fix it by running the following command:

```bash
sudo chown root:root / # fix root
```

### Fix Connection timed out and Transport endpoint is not connected errors

If you are getting the following errors when trying to upgrade your system:

```bash
Failed to get load state of systemd-networkd.socket: Connection timed out
Failed to try-restart systemd-networkd.service: Transport endpoint is not connected
```

Try running the [following commands](https://forum.proxmox.com/threads/updating-lxc-debian-10-to-11-issues.101583/post-500119):

```bash
sudo mv /etc/acpi/events /etc/acpi/events.bak
sudo kill -HUP 1
```
