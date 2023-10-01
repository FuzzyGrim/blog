---
layout: post
title: "Connecting to a VPN provider with WireGuard"
date: 2021-11-26
excerpt: "Friendship ended with OpenVPN, now WireGuard is my best friend!"
tags: [tutorial, privacy, vpn]
---

I recently decided to try out Windscribe, although they currently don't have an app for Arch Linux. They offer OpenVPN, IKEv2 and WireGuard configuration files. I have always used OpenVPN but this time, I wanted to try WireGuard, a fairly new protocol that promises to be simpler, more secure, and faster than the alternatives. All the guides I found were about setting up a home VPN server with WireGuard, however, connecting to a VPN provider is much simpler. I am writting this as a reference note in case I have to set it up again, and if I can help someone else, all the better.

## Wireguard installation

The official [installation page](https://www.wireguard.com/install/) has the instructions on how to download it in many operating systems. In my case, it is: `sudo pacman -S wireguard-tools`.

## Connecting to a VPN

After getting the WireGuard configuration files from the Windscribe website, move this file to `/etc/wireguard` and you will probably need to rename it because the maximum length of a Linux interface name seems to be 15 characters, for example: `sudo mv Windscribe-Copenhagen-LEGO.conf /etc/wireguard/copenhagen.conf`.

Once this is done, you are ready to connect to the server, do it by simply running the command: `sudo wg-quick up copenhagen`, and to disconnect, you can use: `sudo wg-quick down copenhagen`.

---
**⚠ INFO:**

If you are getting something like:

> `/usr/bin/wg-quick: line 32: resolvconf: command not found`

On Arch Linux, you will need to install the `systemd-resolvconf` package.

On Debian, you will need to install the `openresolv` package.

---

Finally, if you want to configure the VPN interface to be persistent across reboots, you can do so by enabling it as a service with: `sudo systemctl enable wg-quick@copenhagen`.
