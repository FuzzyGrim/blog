---
layout: post
title: "My self-hosting setup"
date: 2022-01-22
excerpt: "An overview of my first self-hosted setup with the hows and whys."
tags: [self-hosting]
comments: true
---

A few months ago, I [started my journey](https://www.fuzzygrim.com/posts/jellyfin) in self-hosting with a media server on my laptop, and after playing with some other applications and being able to enjoy the control over my system and my data, I became even more interested. Also, as I started college, I have needed my laptop for it, so I decided to buy a dedicated machine for hosting and take this opportunity to learn more.

So, I am writing this to share my current setup after tinkering during my year-end break as a way to document what I did for myself and for others who may be interested. Also, with this, I will have a record of what I did during this moment in time to look back on in the future.

## Hardware  
My budget was around 300€, and I was looking for something small and quiet because I was going to place it in my bedroom, another plus was to have a low power consumption (0.27 €/kWh). My initial idea was to buy a Raspberry Pi, but after searching for it, I think they are quite expensive for what they offer. My next idea was a Intel NUC, and as I was looking for a good deal on Ebay, I found something even better in my opinion, a [Fujitsu C720](https://www.fujitsu.com/es/products/computing/pc/desktops/esprimo-c720/) for 65€ with the following specifications:

- Intel Core i5-4570
- 8GB RAM DDR3
- 1TB HDD

I wanted an SSD for the OS, and since the PC didn't seem to have space for another internal drive, I bought a 500GB external SSD from [Samsung](https://www.amazon.es/Samsung-T5-500GB-Estado-Externo/dp/B074MCM721) for 75€. The 1TB hard drive that came with the PC was going to be used for NextCloud and I already had a 1TB external hard drive for backups. Finally, I wanted another drive to store my media server, so I went with a 2TB [WD Elements](https://www.amazon.es/WD-Elements-Disco-Externo-port%C3%A1til/dp/B06W55K9N6) for 60€. This added up to a total price of 200€.

![Photo 22-01-24 13-50-54 6159](https://user-images.githubusercontent.com/34800654/150787370-f90ab96f-6319-4545-a88f-d943804ad747.jpg)

## Operating System
At first, I was thinking of using Debian or Ubuntu Server, however after reading different posts on [r/homelab](https://www.reddit.com/r/homelab) and [r/selfhosted](https://www.reddit.com/r/selfhosted/) I was convinced to try Proxmox. My current setup, after learning how to use Proxmox and seeing multiple comparisons on VM vs LXC, is:

![Proxmox](https://user-images.githubusercontent.com/34800654/150637015-4bee6673-58db-4262-b919-a60d87c570f8.png)

I basically have 4 LXC containers:

- Resettable: A container that I don't mind being restarted/downloaded, it currently hosts  [Libreddit](https://github.com/spikecodes/libreddit), [Invidious](https://github.com/iv-org/invidious), [LibreSpeed](https://github.com/linuxserver/docker-librespeed/) and [Portainer](https://www.portainer.io/).

- Media Server: Here I am hosting [Jellyfin](https://jellyfin.org/), [Radarr](https://radarr.video/), [Sonarr](https://sonarr.tv/), [Bazarr](https://www.bazarr.media/) and [Ombi](https://ombi.io/) and I have binded the 2TB external HDD.

- VPN: Some services that needs to be run behind a VPN, they are currently only [Transmission](https://transmissionbt.com/) and [Prowlarr](https://github.com/Prowlarr/Prowlarr), it has also binded the 2TB external HDD.

- Storage: All the services whose data is stored on the 1TB hard disk and needs to be backed up to the 1TB external hard disk, I host most of my services here and some of them are: [Vaultwarden](https://github.com/dani-garcia/vaultwarden), [Nextcloud](https://nextcloud.com) and [Bookstack](https://www.bookstackapp.com/).

## Services

I am running all my services as a [Docker](https://www.docker.com/) container with [docker compose](https://docs.docker.com/compose/), which made it easier to maintain all the different services I was hosting. Each group of related services has its own docker-compose.yml with all the services you need, such as databases or auto-healing. You can find all my docker-compose files on [Github](https://github.com/FuzzyGrim/docker-compose).

I am currently hosting over 20 services, which are currently only meant to be used by myself and therefore are not faced to the public. Some of the services I want to highlight are shown below with their docker-compose file configuration.

### Heimdall

A simple and beautiful dashboard, I use it mainly as a way to see all my current hosted services and as a shortcut to each of them.

You can see most of the services I am hosting with my Heimdall dashboard:

![Heimdall](https://user-images.githubusercontent.com/34800654/150653597-a592ceea-98f4-4f7a-a801-31574e4ef95a.png)

```yml
version: "2.1"
services:
  heimdall:
    image: lscr.io/linuxserver/heimdall
    container_name: heimdall
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Madrid
    volumes:
      - /data/heimdall:/config
    ports:
      - 49154:80
      - 49155:443
    restart: unless-stopped
```

### Media Server
The main reason I wanted to start self-hosting, it can automatically download the necessary episodes as they are released.

![Jellyfin](https://user-images.githubusercontent.com/34800654/150637727-4792757b-84b3-4b73-ade7-a85478cdaa10.png)

It is composed by a stack with Jellyfin, Sonnar, Radarr, Bazarr and Ombi and another stack with Prowlarr and Transmission because they are hosted in different LXC containers:

```yml
version: "3"
services:
  jellyfin:
    image: linuxserver/jellyfin
    container_name: jellyfin
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Madrid
    volumes:
      - ./jellyfin_config:/config
      - /data/media/tvshows:/data/tvshows
      - /data/media/movies:/data/movies
      - /data/media/anime:/data/anime
    devices:
      - "/dev/dri:/dev/dri"
    ports:
      - 8096:8096
    restart: unless-stopped

  sonarr:
    image: linuxserver/sonarr
    container_name: sonarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Madrid
    volumes:
      - ./sonarr_config:/config
      - /data/media/anime:/anime
      - /data/media/tvshows:/tvshows
      - /data/transmission/downloads/complete:/downloads/complete
    ports:
      - 8989:8989
    restart: unless-stopped

  radarr:
    image: linuxserver/radarr
    container_name: radarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Madrid
    volumes:
      - ./radarr_config:/config
      - /data/transmission/downloads/complete:/downloads/complete
      - /data/media/movies:/movies
    ports:
      - 7878:7878
    restart: unless-stopped

  ombi:
    image: linuxserver/ombi
    container_name: ombi
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Madrid
    volumes:
      - ./ombi_config:/config
    ports:
      - 3579:3579
    restart: unless-stopped
    depends_on:
      - radarr
      - sonarr
  bazarr:
    image: linuxserver/bazarr
    container_name: bazarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Madrid
    volumes:
      - ./bazarr_config:/config
      - /data/media/movies:/movies #optional
      - /data/media/tvshows:/tvshows #optional
      - /data/media/anim:/anime
    ports:
      - 6767:6767
    restart: unless-stopped
```

```yml
version: "2.1"
services:
  prowlarr:
    image: linuxserver/prowlarr:develop
    container_name: prowlarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Madrid
    volumes:
      - ./prowlarr:/config
    ports:
      - 9696:9696
    restart: unless-stopped

  transmission:
    image: linuxserver/transmission
    container_name: transmission
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Madrid
    volumes:
      - ./transmission:/config
      - /data/transmission/downloads:/downloads
    ports:
      - 9091:9091
      - 51413:51413
      - 51413:51413/udp
    restart: unless-stopped
```

### Data analytics and monitoring

I don't monitor a lot of data and I don't look at it very often but I think it came out pretty good.

![Grafana](https://user-images.githubusercontent.com/34800654/150647728-ade4fc35-0073-4c96-93f6-0abb0b08e93f.png)

Although the full configuration of the monitoring stack is beyond the scope of this post, the main docker-compose file is a stack with Grafana, Loki, Promtail and Prometheus. I install Netdata separately with this command: `bash <(curl -Ss https://my-netdata.io/kickstart.sh)`.

```yml
version: "3"
networks:
  loki:
services:
  loki:
    image: grafana/loki:2.4.0
    volumes:
      - /data/loki:/etc/loki
    ports:
      - "3100:3100"
    restart: unless-stopped
    command: -config.file=/etc/loki/loki-config.yml
    networks:
      - loki

  promtail:
    image: grafana/promtail:2.4.0
    volumes:
      - /var/log:/var/log
      - /data/promtail:/etc/promtail
    # ports:
    #   - "1514:1514" # this is only needed if you are going to send syslogs
    restart: unless-stopped
    command: -config.file=/etc/promtail/promtail-config.yml
    networks:
      - loki

  grafana:
    image: grafana/grafana:latest
    user: "1000"
    volumes:
    - /data/grafana:/var/lib/grafana
    ports:
      - "3000:3000"
    restart: unless-stopped
    networks:
      - loki

  prometheus:
    container_name: prometheus      
    ports:
      - '9080:9090'
    volumes:
      - '/data/prometheus:/etc/prometheus'
    image: prom/prometheus
    restart: unless-stopped
    networks:
      - loki
```

### Bookstack

An elegant wiki software, I use it for documentation and note-taking.

![BookStack](https://user-images.githubusercontent.com/34800654/150647786-711a033e-afd7-4a91-8b35-ab716da203b7.png)

```yml
version: "2"
services:
  bookstack:
    image: linuxserver/bookstack
    container_name: bookstack
    environment:
      - PUID=1000
      - PGID=1000
      - APP_URL=${URL}
      - DB_HOST=bookstack_db
      - DB_USER=bookstack
      - DB_PASS=${DB_PW}
      - DB_DATABASE=bookstackapp
    volumes:
      - /data/bookstack/config:/config
    ports:
      - 6875:80
    restart: unless-stopped
    depends_on:
      - bookstack_db
  bookstack_db:
    image: linuxserver/mariadb
    container_name: bookstack_db
    environment:
      - PUID=1000
      - PGID=1000
      - MYSQL_ROOT_PASSWORD=${DB_PW}
      - TZ=Europe/Madrid
      - MYSQL_DATABASE=bookstackapp
      - MYSQL_USER=bookstack
      - MYSQL_PASSWORD=${DB_PW}
    volumes:
      - /data/bookstack/database/config:/config
    restart: unless-stopped
```

### Vaultwarden
It's an unofficial server implementation of Bitwarden. It's a fast, simple and cross-platform password manager.
```yml
version: '3'

services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: always
    environment:
      - WEBSOCKET_ENABLED=true  # Enable WebSocket notifications.
    volumes:
      - ./vw-data:/data
    ports:
      - 8086:80
```
### Nextcloud
Probably the most popular self-hosted application, I'm using it for cloud storage, CalDAV, CardDAV, Kanban, Bookmarks and Notes. Although I've seen people complaining about the slowness of the software and the difficulty in configuring it, I haven't had any problems with my hardware.

![Nextcloud](https://user-images.githubusercontent.com/34800654/150647833-b7359354-c000-4cb0-9da7-b1b20cbe878f.png)

```yml
version: "3.7"

services:
  nextcloud:
    image: linuxserver/nextcloud:latest
    container_name: nextcloud
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Madrid
    volumes:
      - /data/nextcloud/config:/config
      - /data/nextcloud:/data
    ports:
      - 9443:443
    restart: unless-stopped
    depends_on:
      - db

  db:
    image: linuxserver/mariadb:latest
    container_name: nextcloud_db
    environment:
      - PUID=1000
      - PGID=1000
      - MYSQL_ROOT_PASSWORD=${DB_ROOT_PW}
      - TZ=${TIMEZONE}
      - MYSQL_DATABASE=${DB_NAME}
      - MYSQL_USER=${DB_USER}
      - MYSQL_PASSWORD=${DB_PW}
    volumes:
      - /data/nextcloud/config_db:/config
    restart: unless-stopped
```
### Duplicati
Duplicati is my go-to backup tool, I use it to back up to my 1TB external drive and MEGA.NZ as I currently don't have much data to back up. I am thinking of using BlackBlaze B2 in the future once my data is big enough.

![Duplicati](https://user-images.githubusercontent.com/34800654/150647822-6274cd5e-1b64-41d7-bb50-b3ded8f58ec7.png)

```yml
version: "2.1"
services:
  duplicati:
    image: lscr.io/linuxserver/duplicati
    container_name: duplicati
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Madrid
    volumes:
      - ./config:/config
      - /backup:/backups
      - /data:/source
    ports:
      - 8200:8200
    restart: unless-stopped
```

### Watchtower
Watchtower is great for keeping your services up to date, although the default setting is to update them automatically, I prefer to simply receive a discord notification when an update is available.

```yml
version: "3"
services:
  watchtower:
    container_name: watchtower
    image: containrrr/watchtower
    restart: unless-stopped
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      TZ: Europe/Madrid
      WATCHTOWER_SCHEDULE: 0 30 18 * * *
      WATCHTOWER_MONITOR_ONLY: "true"
      WATCHTOWER_CLEANUP: "true"
      WATCHTOWER_NO_STARTUP_MESSAGE: "true"
      WATCHTOWER_NOTIFICATIONS: "shoutrrr"
      WATCHTOWER_NOTIFICATION_URL: "discord://token@channel"
```

### Caddy
Caddy is my choice for reverse-proxy, it is used to map a domain or subdomain to the corresponding port exposed in the docker container. It is simple to configure and also automatically obtains and renews TLS certificates for you.

```yml
version: '3'

services:
  caddy:
    image: caddy:2
    container_name: caddy
    restart: always
    ports:
      - 80:80
      - 443:443
    volumes:
      - ./caddy:/usr/bin/caddy  # Your custom build of Caddy.
      - ./Caddyfile:/etc/caddy/Caddyfile:ro
      - ./caddy-config:/config
      - ./caddy-data:/data
    environment:
      - EMAIL=${email}
      - CLOUDFLARE_API_TOKEN=${cloudflare_token}
      - LOG_FILE=/data/access.log
```

You can see the rest of my docker-compose files on [Github](https://github.com/FuzzyGrim/docker-compose).


## Conclusion
As you can see, my current setup is just a combination of multiple Docker services that I take care of, I am considering to learn Ansible for some automation in the future. However, the next rabbit hole will be home automation with Home Assistant.

That's it my friends, if you made it this far, congratulations and thanks for reading. I wish you the best in your self-hosting adventures!