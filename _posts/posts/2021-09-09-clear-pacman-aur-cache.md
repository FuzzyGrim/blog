---
layout: post
title: "Arch linux maintenance recommendations"
date: 2021-09-09
excerpt: "It is never a bad idea to clean your cache from time to time."
tags: [arch linux, tutorial]
---

## Clean the cache in your /home directoy
As we use our system, the cache will fill up and take up a lot of space. The folder dedicated to the cache is `~/.cache`, if you want to check the size of the folder, you can do it with this command:
```
du -sh ~/.cache/
```

## Remove unwanted packages

This command shows all packages you have explicitly installed with their respective memory size: 

```
pacman -Qei | awk '/^Name/{name=$3} /^Installed Size/{print $4$5, name}' | sort -h
```

The same command as the previous but it only shows packages installed from the AUR:

```
pacman -Qim | awk '/^Name/{name=$3} /^Installed Size/{print $4$5, name}' | sort -h
```

Uninstall all unneeded packages and their unused dependencies:
```
sudo pacman -Rsn $(pacman -Qdtq)
```

## Clean pacman cache

You can first check the total number of cached packages with: 
```
sudo ls /var/cache/pacman/pkg/ | wc -l
``` 

And the total disk space used with:
```
du -sh /var/cache/pacman/pkg/
```

To clean the cache created by pacman, we will be using a program called `paccache` which is in `pacman-contrib`. To clean all packages except the 3 most recent versions, run the following command: 

```
sudo paccache -r
```

Pacache also allows you to choose how many recent versions you want to keep, for example, if you just want to keep the latest version, you can use:
```
sudo paccache -rk1
```

You can limit the action of paccache to uninstalled packages, to remove all cached versions of uninstalled packages, use:
```
sudo paccache -ruk0
```

It is also possible to automate paccache. With the following command, `paccache` deletes the cache weekly:
```
sudo systemctl enable paccache.timer --now
```

If you want `paccache` to clean the cache monthly, you will need to create `/etc/systemd/system/paccache.timer` and add:

```
[Unit]
Description=Clean-up old pacman pkg

[Timer]
OnCalendar=monthly
Persistent=true

[Install]
WantedBy=multi-user.target
```

Another option is to create a pacman hook, this will run `paccache` after a determined pacman operation, for example if you want it to run whenever you upgrade, install or remove a package; just create `/etc/pacman.d/hooks/clean_package_cache.hook` and add:

```
[Trigger]
Operation = Upgrade
Operation = Install
Operation = Remove
Type = Package
Target = *
[Action]
Description = Cleaning pacman cache...
When = PostTransaction
Exec = /usr/bin/paccache -rk 1 -ruk 0
```


## Clean system logs

You can check the disk space used by the journal files with:
```
journalctl --disk-usage
```

This can be removed using `rm` on `/var/log/journal/` or by `journalctl`. You can keep only the latest logs by size limit (e.g. keep only 100Mb of the latest logs):

```
journalctl --vacuum-size=100M
```

Or by time limit:
```
journalctl --vacuum-time=7d
```

After cleaning the log files, you can uncomment `SystemMaxUse` in `/etc/systemd/journald.conf` to limit the disk usage used by these files by , e.g: `SystemMaxUse=500M`

## Extra tips and recommendations:

To view the dependency tree of a package, use `pactree` from `pacman-contrib`:

```
$ pactree broot
broot
├─gcc-libs
│ └─glibc provides glibc>=2.27
│   ├─linux-api-headers provides linux-api-headers>=4.10
│   ├─tzdata
│   └─filesystem
│     └─iana-etc
└─zlib
  └─glibc
```
To view the dependent tree of a package, pass the reverse flag `-r` to `pactree`:
```
$ pactree -r xmonad
xmonad
└─xmonad-contrib
```

If you just want a simple list instead of a tree view, you can use `pacman -Qi package-name | grep "Depends On"` and `pacman -Qi package-name | grep "Required By"`

To get a list of the packages you have installed sorted by the date and time, you can use this script:
```
#!/bin/sh
# Loop through packages explicitly installed
for i in $(pacman -Qe)
do
  # Search your installed packages in pacman.log
  grep "\[ALPM\] installed $i" /var/log/pacman.log
done | \
  sort -u | \
  # Clean the output
  sed -e 's/\[ALPM\] installed //' -e 's/(.*$//'
```

Finally, if you want to check which folders are taking a lot of space, I recommend using `ncdu`.


