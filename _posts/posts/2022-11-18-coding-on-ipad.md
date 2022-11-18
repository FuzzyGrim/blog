---
layout: post
title: "Replacing my MacBook with an Ipad"
date: 2022-11-18
excerpt: "VSCode and Neovim setup on Ipad"
tags: [docker, tutorial, programming]
---

Why would you do that, you might ask. Basically, for the last year, I used a MacBookPro for my studies, but its keyboard and trackpad stopped working constantly. Although it seems to be a pretty [common](https://search.brave.com/search?q=fix+macbook+pro+2015+keyboard+trackpad) problem for the laptop I had, I couldn't fix it with the usual trick of changing the ribbon cable. So I decided to try using the 2017 iPad I already used to take my math and physics notes for everything I did on the laptop. Although there were some struggles on the trip, I am satisfied with my current setup, so I decided to write this post to share my experience.

## Hardware

The most important thing I needed to replace a laptop with an iPad was, of course, a keyboard. I had a hard time deciding which keyboard to use, I didn't know if I should go for a [keyboard case](https://www.amazon.com/ipad-keyboard-case/s?k=ipad+keyboard+case) or an external keyboard. There were many options, but I finally decided on the [Logitech K380](https://www.logitech.com/es-es/products/keyboards/k380-multi-device.html), mainly because it was cheap ([30€](https://www.amazon.es/Logitech-K380-Teclado-Bluetooth-Windows/dp/B013SL1ZBK)), has a good battery life and came with the layout I wanted.

I decided not to buy a keyboard case after I had a chance to try one out and found that I didn't like how small the keys were. However, this meant I had to get an iPad case, my main requirements were that it had to have multiple standing positions. There were many options, but I settled on this one (https://www.amazon.es/gp/product/B0979KG3QF).

And last but not least, a server. I tried to program locally on the iPad, but it was really poor in terms of performance and file management. Luckily, I already had a [Proxmox server](https://www.fuzzygrim.com/posts/selfhosted-setup) at home, so I simply created another LXC container and installed Debian on it.

## Code-server

I normally use VSCodium for my university Java projects, as I already had an instance of [code-server](https://github.com/coder/code-server) hosted on my server, which is basically VS Code in the browser, the transition was not difficult. It's fairly simple to set up with docker and works well. This is the docker-compose file I use that has the additional tags for [Traefik](https://www.fuzzygrim.com/posts/exposing-services#traefik):

```yml
version: "2.1"
services:
    code-server:
        image: lscr.io/linuxserver/code-server:latest
        container_name: code-server
        environment:
            - PUID=1000
            - PGID=1000
            - TZ=Europe/Madrid
            - HASHED_PASSWORD=${PASS}
            - SUDO_PASSWORD_HASH=${SUDO}
            - PROXY_DOMAIN=${URL}
            - DEFAULT_WORKSPACE=/config/workspace #optional
        volumes:
            - ./config:/config
            - ./scripts:/custom-cont-init.d:ro
        restart: unless-stopped
        networks:
            - proxy
        labels:
            - "traefik.enable=true"
            - "traefik.http.routers.code-server.entrypoints=https"
            - "traefik.http.routers.code-server.rule=Host(`coder.domain.com`)"
            - "traefik.http.routers.code-server.tls=true"
            - "traefik.http.services.code-server.loadbalancer.server.port=8443"

networks:
    proxy:
        external: true
```

As I use VSCodium for my Java projects, I needed to install java in the container, for LinuxServer images, you can create a script, and it will run when the container starts. I created a script at `./scripts/java17` with the following content:

```bash
#!/bin/bash
sudo apt-get update
apt install -y openjdk-17-jdk openjdk-17-jre
```

It works quite well as a PWA which is what the developers [recommends](https://github.com/coder/code-server/blob/main/docs/ipad.md). However, code-server seems to have a [issue](https://github.com/microsoft/vscode/issues/149048) with iOS floating shortcut bar which forced me to disable it. But anyway, this is an image of what code-server looks like.

![coder-pwa](https://user-images.githubusercontent.com/34800654/202573573-3516d009-724d-462c-aaf3-897cbe1b6139.jpg)

## Neovim

For the rest of my programming needs, I use [neovim](https://neovim.io/), I tested many applications including [iVim](https://apps.apple.com/us/app/ivim/id1266544660) and [iSH](https://apps.apple.com/us/app/ish-shell/id1436902243), however none of them satisfied me and I finally settled for SSHing to my server.

### Blink

My ssh tool of choice is [Blink](https://blink.sh/), which I got a student license by asking them by email. It supports [mosh](https://mosh.org/), an alternative to ssh that was built for better stability, especially in mobile use cases. Basically, it stays connected to the server even if the connection is lost, or after putting the iPad to sleep.

Another great feature of Blink is that it has local storage stored in the Files app, letting you to copy files to your iPad with `scp` or upload them from your iPad. Other than that, it has configurable themes, fonts and customizable keybindings to the level of being able to have different actions for the same key when pressed or if held.

### WG-EASY and DuckDNS

For remote access to my server, I am using [Wireguard](https://www.wireguard.com/), which is an easy-to-configure VPN protocol with good performance. It allows you to remotely access machines on your network as if you were at home.

I am managing Wireguard with [wg-easy](https://github.com/WeeJeWel/wg-easy), it is easy to install with docker and gives you a web GUI to manage your clients, it also has a nice feature that allows you to generate QR codes for your clients, which is useful for mobile devices. 

![wg-easy](https://user-images.githubusercontent.com/34800654/201925082-97a7d155-f158-4d97-bd2d-afe3a12dc6c2.png)

This is my docker-compose file:

```yml
version: "3.8"
services:
    wg-easy:
        environment:
            - WG_HOST=domain.duckdns.org
            - WG_PORT=4500

        image: weejewel/wg-easy
        container_name: wg-easy
        volumes:
            - ./config:/etc/wireguard
        ports:
            - "4500:51820/udp"
            - "51821:51821/tcp"
        restart: unless-stopped
        cap_add:
            - NET_ADMIN
            - SYS_MODULE
        sysctls:
            - net.ipv4.ip_forward=1
            - net.ipv4.conf.all.src_valid_mark=1
```

If you are using Proxmox's LXC, you will need to edit `/etc/pve/lxc/<id>.conf` and add the following line for Wireguard to work:

```bash
lxc.cap.drop:
```

Since I have a dynamic IP, I needed a dynamic DNS for the `WG_HOST` variable, I went with [duckdns](https://www.duckdns.org/) because it is free, easy to configure and reliable. The domain name provided by duckdns will have to point to your current home IP, so you will need to have something to update it, I am using the following docker container:

```yml
version: "2.1"
services:
    duckdns:
        image: lscr.io/linuxserver/duckdns:latest
        container_name: duckdns
        environment:
            - PUID=1000
            - PGID=1000
            - TZ=Europe/Madrid
            - SUBDOMAINS=${SUBDOMAIN}
            - TOKEN=${TOKEN}
            - LOG_FILE=true
        volumes:
            - ./config:/config
        restart: unless-stopped
```

After this configuration, I was able to access my server with 4G outside my home network. However, when I tried to access from my university network, I found that Wireguard was blocked, after searching for solutions for a while, I found out about the [eduroam rules](https://www.eduroam.org/wp-content/uploads/2020/02/GN3-12-192_eduroam-policy-service-definition_ver28_26072012.pdf), which basically prohibits blocking UDP ports 4500, 1194 and 500. Since my university uses eduroam, I tried changing my `wg-easy` container port to 4500 and it worked.

### Neovim on debian

As I have Neovim configured with Lua, I needed at least version 0.5. but in the Debian repositories they had a very old version of it (0.4.4-1). Therefore, I needed to install it in a different way, as I dislike to install things from source, I decided to use the AppImage version. You can download the 0.8 version by running:

```bash
wget https://github.com/neovim/neovim/releases/download/v0.8.0/nvim.appimage
```

After downloading it, you will need to make it executable:

```bash
chmod u+x nvim.appimage
```

And finally, you can run it with:

```bash
./nvim.appimage
```

However, I ran into some bugs related to FUSE, good thing that Neovim's Github page provided a solution by extracting the appimage, which can be done with:

```bash
./nvim.appimage --appimage-extract
```

This created an executable in `./squashfs-root/usr/bin/nvim`, as I wanted to start Neovim with just `nvim`, I created a symbolic link to it at `/home/user/bin/nvim` so I could use it from anywhere, this is the command I used for reference:

```bash
ln -s /home/user/appimages/squashfs-root/usr/bin/nvim /home/user/bin/nvim
```

Of course, you will need to have that folder in your `$PATH`, if you haven't already done so, you can add the following line to your `.bashrc`:

```bash
export PATH="/home/user/bin:$PATH"
```

### Tmux

As a final touch, as a window manager user, I wanted to have something similar on my setup, basically a way to have multiple panels in a single window. I decided to try [tmux](https://github.com/tmux/tmux), which seemed to be the most popular solution. You can install it by running:

```bash
sudo apt install tmux
```

To enable mouse/finger support, add the following line to your `.tmux.conf`:

```bash
set -g mouse on
```

Also, Tmux is at its core a terminal multiplexer, giving you control over multiple persistence sessions. This means that you can have multiple sessions, each with its own set of windows and panels that will be saved even when you close Blink. You can add the following line to your `Command` section in your `Host` configuration in Blink to start Tmux automatically when you connect to your server:

```bash
tmux new -A -s session
```

I had a [problem](https://github.com/neovim/neovim/issues/7764) with Tmux not respecting the background color of my Neovim configuration, and all the solutions I found on internet didn't work for me. As I use the [Dracula](https://draculatheme.com) colorscheme for Neovim, my workaround was to install Dracula in Tmux itself and set the background color in Neovim to `none`, which is the default color. As I use [AstroVim](https://astronvim.github.io/), I did this by adding the following code to my `user/init.lua`:

```lua
local config = {
    highlights = {
        dracula = {
            Normal = { bg = none },
        }
    }
}
```


After this, I just needed to clone my [dotfiles](https://github.com/fuzzyGrim/dotfiles) and install the plugins with `:PackerSync` and I was ready to go. Here is a screenshot of my neovim setup with tmux. Note that I have zoomed in a bit to make the image font readable on the blog, but this is what I usually [see](https://user-images.githubusercontent.com/34800654/202558304-5a4bf789-6257-4ff5-a6b6-6a6fabfa94a1.jpg) on my iPad:

![nvim-up](https://user-images.githubusercontent.com/34800654/202556944-45aa85e7-c429-4170-b608-60cb73b361ef.jpg)

## Final thoughts

I have been using this setup for a month now, and it has worked quite well for me, Neovim has worked smoothly, and I have learned about Tmux which I will probably use more in the future.

However, there have been tradeoffs, for example, sometimes I find myself wishing for more screen space for multitasking. Another big problem I'm trying to solve is having a shared clipboard between iOS and my server. Another "problem" with programming on iPad is that, as I've said, you'll need a server or cloud instance to do most of your work, as there's not much you can do locally due to the lack of good native editors.

Overall, while it's not perfect, I'm happy with the setup, as I think it has done a good job of replacing my laptop for most of my college work. I think it's a perfect mobile device for coding on the go, but it really can't replace a primary development machine.

I hope you found this guide helpful and if you have any questions or comments, feel free to drop me an [email](mailto:fuzzygrim@protonmail.com) or send me a message with Matrix at [@fuzzygrim:matrix.org](https://matrix.to/#/@fuzzygrim:matrix.org).