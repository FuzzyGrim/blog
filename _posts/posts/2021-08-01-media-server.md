---
layout: post
title: "Media server setup with Jellyfin, Sonarr, Radarr and Prowlarr"
date: 2021-08-01
excerpt: "Setting up a home media server may sound intimidating, but it doesn’t have to be."
tags: [self-hosting, foss, tutorial]
redirect_from:
  - /posts/jellyfin
---

## What is Jellyfin?

Jellyfin is a client/server media player system. When you install a media server program on your computer, it becomes your host server. You can connect to it from your phone and stream the media that's stored on your computer's hard drive. Think of it like Netflix, but your computer is the server and the content that is available is based on the media files on the computer. Another more known media server is Plex, but Jellyfin is a completely free and open-source alternative. Jellyfin is available for Linux, MacOS and Windows.

## What is Sonnarr, Radarr and Prowlarr?

You know how you have to manually download the episodes of the shows you like and rename them correctly? Well these three apps will help you automate it. They automatically download new episodes as they become available and can replace lower quality videos with higher quality ones once they become available. Sonarr is for series and Radarr for movies. Prowlarr is an application that will help Sonarr and Radarr to search for different series and movies from different trackers. It allows you to add all your favorite torrent indexing sites in one place without the need to visit each site individually every time you go to search for a series or movie.

## Jellyfin configuration
Install the latest Jellyfin server from [here](https://jellyfin.org/downloads/). On MacOS, remember to move the application to the Application folder. On Arch Linux, you can install Jellyfin from the AUR as [jellyfin-bin](https://aur.archlinux.org/packages/jellyfin-bin/) and start the service with this command: <code>sudo systemctl enable --now  jellyfin.service</code>

After installing Jellyfin, you should first find out the IP of your server. On MacOs you can find it by going to System Preferences > Network > Advanced > TCP / IP > IPv4 Address. It's usually 192.168.1.__ in my case it's 192.168.1.38.

Now start Jellyfin and navigate in your browser to <http://your-server-ip:8096>. You should see an output like the following: 

![intro](https://user-images.githubusercontent.com/34800654/127638294-1a55f7bc-203b-4f1d-8c64-57cea243a151.png)

Choose your language and click next. On the next screen, create a user and enter a password of your choice. The next page asks you to set up your media libraries. If you already have something downloaded, you can import it here. Choose the type of content (i.e. audio, video, movies etc.), enter the display name and click the plus (+) sign next to the folders icon to choose the location to save your media files.

![media](https://user-images.githubusercontent.com/34800654/127638456-629794ac-c89d-4535-b576-0ee052431ef6.png)

Then click next to all of the following prompts and once you log in, you should see your previously imported media files.

---
**⚠ WARNING:** If the subtitles aren't showing, go to your media folder and run:

```
find . -iname '*.srt' -print -exec sed -i '1s/^/\xef\xbb\xbf/' {} \;
```

It will insert a UTF-8 [byte order mark](https://wikiless.org/wiki/Byte_order_mark?lang=en) into the subtitle files, which tells it the specific text format of the file.

---

If you want to change anything or reconfigure, click on the three horizontal bars from the Home screen and go to Dashboard. I would recommend adding a new user for your guests, to do this, click the add button and create a new user, you can create a new user without a password if you want.

![guest](https://user-images.githubusercontent.com/34800654/127638487-ca3fb107-4c39-4874-8915-cd34aefb6723.png)

You can access this Jellyfin media server from any systems or device such as mobile phone, tablet or pc by going to <http://your-server-ip:8096>. 

## Sonarr, Radarr and Prowlarr configuration

Download [Sonarr](https://sonarr.tv/#download), [Radarr](https://radarr.video/#download) and [Prowlarr](https://wiki.servarr.com/prowlarr/installation). On MacOS, remember to move the applications to the Application folder. You will need to start the services with your tool of choice. For example, on MacOS by opening Sonarr.app in your Application folder or on ArchLinux with this command: <code>sudo systemctl enable --now  app-name.service</code>

We will start by configuring Sonarr and Radarr, Sonarr is hosted at <http://your-server-ip:8989> while Radarr is at <http://your-server-ip:7878>. Both have the same settings and you will probably want something similar to my settings. For a more detailed breakdown of all the settings you can consult the [Radarr wiki](https://wiki.servarr.com/radarr/settings) and the [Sonarr wiki](https://wiki.servarr.com/sonarr/settings).

To start configuring these two apps, go to Settings > Media Management:

Click on "Show advanced":

![show-advanced](https://user-images.githubusercontent.com/34800654/127638582-91321adb-44e7-4d6c-a504-faa02325a5bc.png)

And here are my settings for reference:

![media-management](https://user-images.githubusercontent.com/34800654/127642257-f6d2a2d7-36cc-4e73-ae36-9f0863e20838.png)

![media-management-2](https://user-images.githubusercontent.com/34800654/127642314-e494fc8f-15f6-4c97-9f1b-c4788366f12f.png)

![media-management-3](https://user-images.githubusercontent.com/34800654/127642834-74f8c34c-b370-46cf-92c0-4c6ed6e79ba1.png)

![media-management-4](https://user-images.githubusercontent.com/34800654/127642447-6d2e3976-1ce1-41f3-bef7-bb7351077d32.png)

Remember to do the same in both applications. If you prefer to watch your media in the original language, in Radarr go to Settings > Profiles and change "Language" to Original, however this option isn't available in Sonarr. Here you can also decide if you want these two apps to download a higher quality video once it's available:

![profile](https://user-images.githubusercontent.com/34800654/127638683-5eaa844f-578c-4868-bc6b-997d00030b18.png)

<hr>

Now let's configure Prowlarr, once installed, launch the application and go to <http://your-server-ip:9696>. You should be see something like this:

![prowlarr-intro](https://user-images.githubusercontent.com/34800654/127785792-4a61c9f0-97bf-4200-aea0-b7c774e97b1a.png)

Now to add your favorite sites, click on the button that says "Add Indexer" and search for your desired sites. You can usually leave all the fields with the default value and test if everything is working correctly with the button at the bottom before saving it.

To sync these indexers with Sonarr and Radarr, in Prowlarr go to the Settings > Apps, click the big plus (+) button under "Applications"and select Sonarr. Fill in the Apikey which can be found on the Sonarr site at <http://your-server-ip:8989> in Settings > General, and leave the rest as it is. Once done you can test the configuration and, if everything looks fine, save it. Remember to do the same with Radarr at <http://your-server-ip:7878>. After adding the two apps into Prowlarr, go to Radarr or Sonarr and you should see your indexers at Settings > Indexers.

Finally, the last thing to configure will be the torrent client, I will be using [Transmission](https://transmissionbt.com/download/), but you can use your preferred torrent client. You will need to allow external connections in Transmission, to do this, go to Preferences > Remote and check the box that says "Allow remote access", you can also set a username and password there but you can leave it blank if you wish.

![transmission](https://user-images.githubusercontent.com/34800654/127638756-f2ea8112-8171-4db8-ae03-2184a64c1d3e.png)

After doing that, go to Settings > Download Clients in Sonarr and Radarr, click on the add button and select Transmission or your preferred BitTorrent client. There you will put a name like "Transmission" and change the Host to your server IP. You can test it to see if it's working.

Now, you can import your library in Movies/Series > Library Import. After importing your library, it's time to add some movies or shows you want, to do that, go to Movies/Series > Add New and search for your shows or movies. Finally, for your Series, go to Series > select your added show and click on the search button for the season you want to download. You can also search manually by clicking on the person icon next to the search icon.

![download-series](https://user-images.githubusercontent.com/34800654/127644257-55aa3ac6-743b-4d8a-a485-7918313dd5c9.png)

## Wrapping Up

Jellyfin is an amazing media server software solution that is free and open source, and with the help of Sonarr, Radarr and Prowlarr, you can host a media server that is easy to maintain. I hope this tutorial has helped you install Jellyfin media server on your system and introduced you to the world of self-hosting if you weren't already in it. If you liked this post or found it useful, I just added an RSS button that should be right below this paragraph so you can receive notifications of upcoming posts.
