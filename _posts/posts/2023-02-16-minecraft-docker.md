---
layout: post
title: "Hosting a Minecraft Server with Docker"
date: 2023-02-16
excerpt: "A simple guide to setup your minecraft server with a lightweight proxy"
tags: [keyboards]
---

Although I have hosted a Minecraft server before, it was quite some time ago when I wasn't into selfhosting, I remember watching some random youtube tutorial and opening a port without thinking twice about the security implications. Now, I got interested in Minecraft again and wanted to host a server for me and my friends, but this time hosting it the right way.

## Minecraft Server

To keep things simple and organized, I like to host things with Docker, so I started looking for a Docker image for Minecraft. After some research, I found [itzg/minecraft-server](https://github.com/itzg/docker-minecraft-server) which seemed to be simple to use, but easily extensible and customizable. To start a basic server, just run the following command:

```
docker run -d -it -p 25565:25565 -e EULA=TRUE itzg/minecraft-server
```

But if you want more, you can check the [README](https://github.com/itzg/docker-minecraft-server) to see all the available options. I am using the following configuration:

```yml
version: "3"

services:
    mc:
        image: itzg/minecraft-server
        container_name: minecraft-server
        ports:
            - 25565:25565
        environment:
            EULA: "TRUE"
            TYPE: "FORGE"
            DIFFICULTY: "normal"
            VERSION: "1.19.2"
            FORGE_VERSION: "43.2.0"
            MEMORY: "8G"
        tty: true
        stdin_open: true
        restart: unless-stopped
        volumes:
            - ./data:/data
            - ./mods:/mods
            - /etc/timezone:/etc/timezone
```

I am using Forge to add some mods to the server, but you can use any other type of mod loader. I needed to map the timezone volume because without it, I was getting `Invalid signature for profile public key` when trying to join the server.

## Exposing it to the internet

As I said before, I didn't want to open any ports to the internet, and Cloudflare Tunnel didn't seem to work with Minecraft, so I decided to use a VPN between my server and a VPS and then expose the server to the internet through the VPS. I am using [Wireguard Easy](https://github.com/WeeJeWel/wg-easy) to configure and manage my VPN, as I explained in one of my [previous post](https://www.fuzzygrim.com/posts/coding-on-ipad#wg-easy-and-duckdns). 

After connecting to the VPN and checking that it was working, the next step was to use a reverse proxy to forward Internet traffic to the VPN. I normally use [Traefik](https://doc.traefik.io/traefik/getting-started/quick-start/) but after trying and failing for a whole day to get it to work with Minecraft, I decided to look for other options. Luckily, it seems that the creator of the Minecraft Server image also has an image for a lightweight proxy for minecraft, [itzg/mc-router](https://github.com/itzg/mc-router), who is this person and why is he so awesome? Anyway, I decided to give it a try and it worked like a charm. This is the `docker-compose.yml` file I am using:

```yml
version: "3.4"

services:
    router:
        image: ${MC_ROUTER_IMAGE:-itzg/mc-router}
        environment:
            # enable API
            API_BINDING: ":25564"
        ports:
            - 25565:25565
            # bind the API port to only loopback to avoid external exposure
            - 127.0.0.1:25564:25564
        command: --mapping=minecraft.example.com=192.168.x.x:25565
```

The only thing you have to change is the `command` line, where you have to replace `minecraft.example.com` with your domain and `192.168.x.x` with your server IP. You can also change the port if you are using a different one.

The last step would be to configure the DNS, as always, add an `A record` pointing to the IP of your VPS and after that, you will have to create a `SRV record` with the following values:

```
Name: minecraft
Service: _minecraft
Protocol: TCP
TTL: Automatic
Priority: 0
Weight: 0
Port: 25565
Target: minecraft.example.com
```

Once you have these two records, you should be able to connect to your server with the domain you set.

## Last words

Now that the server is accessible from the internet, you probably want a whitelist to prevent random people from joining your server. You can `exec` on the container to access the Minecraft server console, for example, to activate the whitelist and add a player you can execute the following commands:

```
docker exec mc rcon-cli whitelist on
docker exec mc rcon-cli whitelist add <player_name>
```

Finally, I recommend checking out some of the [related projects](https://github.com/itzg/docker-minecraft-server#related-projects) from the creator of the Minecraft Server container, he has an image for [minecraft backups](https://github.com/itzg/docker-mc-backup) and an image for hosting [the bedrock edition](https://github.com/itzg/docker-minecraft-bedrock-server) as well.