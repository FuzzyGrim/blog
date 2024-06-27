---
layout: post
title: "Backups with Kopia via Docker"
date: 2024-02-27
excerpt: "Simple and secure 3-2-1 backups with Kopia"
tags: [self-hosting, tutorial]
---

My previous backup solution was [Duplicati](https://duplicati.com/), it was always a bit slow but tolerable. However, after a while, it started to show some errors related to database corruption. I tried to fix it but it seemed to be a recurring problem and was a common complaint on the forums.

After searching for alternatives, I decided to give [Kopia](https://kopia.io/) a try. And after using it for almost a year, I can say that I am very happy with it. It is fast and reliable, I have restored many files with it and it worked perfectly. I am writing this post to show how to set up Kopia with Docker, as I couldn't find a good guide when I was setting it up.

## Docker Compose

```yml
version: "3.7"
services:
    kopia:
        image: kopia/kopia:latest
        hostname: kopia
        container_name: kopia
        restart: unless-stopped
        ports:
            - 51515:51515
        environment:
            - KOPIA_PASSWORD=${KOPIA_REPOSITORY_PASSWORD}
            - TZ=Europe/Madrid
        volumes:
            # Mount local folders needed by kopia
            - ./config:/app/config
            - ./cache:/app/cache
            - ./logs:/app/logs
            - /data:/data:ro
            - /backup:/backup
            - ./export:/export
        command:
            - server
            - start
            - --insecure
            - --address=0.0.0.0:51515
            - --server-username=${KOPIA_USER}
            - --server-password=${KOPIA_USER_PASSWORD}
```

When you start the container, you will have a Kopia server running on port 51515. It will ask you for a username and password, which you can set with the `KOPIA_USER` and `KOPIA_USER_PASSWORD` environment variables. The `KOPIA_REPOSITORY_PASSWORD` is the password for the repository which will be used later. The `data` volume is where you will mount the data you want to back up, and the `backup` volume is where the backups will be stored. The `export` volume is the path in which you want to export the backups.


## Creating a repository for local backups

Once logged in, you will be presented with the following options for creating a repository:

![Screenshot 2024-06-26 at 23-15-23 KopiaUI v0 17 0](https://github.com/FuzzyGrim/Yamtrack/assets/34800654/a61a6f28-afef-46c1-bcac-fee54513737d)

Here is their definition of a repository:

> Kopia allows you to save your encrypted backups (which are called snapshots in Kopia) to a variety of storage locations, and in Kopia a storage location is called a repository.

One downside of Kopia in Docker is that it only supports one repository (one backup location) per container. So if you want to back up to different locations, like local and offsite, you will need to create a new container for each repository.

To back up to a local directory, select the `Local directory or NAS` option. You will be asked for the path where you want to store the backups, which should be `/backup` if you are using the same volumes as in the Docker Compose file. When asked for the repository password, use the value of the `KOPIA_REPOSITORY_PASSWORD` environment variable.

Now for this repository, you will be able to define multiple snapshots, with different paths, schedules, retention policies, etc. 

## Defining a snapshot

To create a snapshot, you will need to define a source, a schedule, and a retention policy. Start by clicking on the `New Snapshot` button on the `Snapshots` tab. You will be asked for the source, which should be `/data` if you are using the same volumes as in the Docker Compose file. 

Next, you will be asked for the different policies, like the schedule, retention, and the name of the snapshot. You can also define filters to exclude files or directories from the snapshot.

![Screenshot 2024-06-26 at 23-41-36 KopiaUI v0 17 0](https://github.com/FuzzyGrim/Yamtrack/assets/34800654/f9624536-0d57-464a-9a66-ab10ca0d6386)

For each setting, you will see your defined value and the current setting (normally the values from the global policy). There are many settings you can define, making Kopias very flexible and powerful. You can leave most of them as they are, but I recommend setting a compression algorithm, as it can save a lot of space. You can check out their [official benchmark](https://kopia.io/docs/advanced/compression/#algorithm) to see which one is the best for you, if you just want a balance between speed and compression, I recommend `pgzip`.

Likewise, you should probably set a schedule for the snapshot, I have mine set to run every day at 3 am. For this, you just need to write `3:00` in the "Times Of Day" field. You can use "Snapshot Actions" and "Folder Actions" to run scripts, for example for database dumps or stopping services before the snapshot.

## Restoring a snapshot

Ok, so you have your backups running, but how do you restore them? If you have lost your server and need to restore the data to a new one, you will need to install Kopia on the new server with the same variables in the Docker Compose file. 

On the main page, select the storage type from where your backups are stored. For example, for the local directory, you will need to select `Local directory or NAS` and provide the path to the backups. Kopia will detect that there are backups in that path and will ask you for the repository password. After inputting the password, you will be able to see all the snapshots.

You can restore the whole snapshot or just some files, and you can also restore them to a different path. To do so, you will need to select the snapshot and go to the path where you want to restore the files from. Once there, you can click on the `Restore Files & Directories` button and enter the destination path. This can be `/data` with the overwrite option enabled if you want to restore the files to the original path or the dedicated `/export` path if you want to restore them to a different location.
