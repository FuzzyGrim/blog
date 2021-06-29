---
layout: post
title: "List of free privacy-conscious apps"
date: 2021-06-21
excerpt: "I don't have anything to hide, but I don't have anything to show you either."
tags: [privacy, foss]
---

This post includes a curated list of apps and tools that I recommend. All the apps listed here are open-source, privacy respecting and  free as [freeware](https://en.wikipedia.org/wiki/Freeware) AND [freedom](https://en.wikipedia.org/wiki/Free_software).

With growing concerns about online privacy and personal data security, more people than ever are considering alternatives to products from large proprietary companies. The main purpose of this post is to show some **easy to set up and use alternatives** to help those who are just starting to switch.


## Social Media

[Invidious](https://yewtu.be/feed/trending) - Alternative and privacy respecting YouTube front-end. Lightweight, supports RSS feeds, JavaScript is optional and no ads.

[Nitter](https://nitter.net/) - Nitter is a free and open source alternative Twitter front-end focused on privacy. Lightweight and faster than Twitter, no JavaScript, no ads and it has RSS feeds.

[Libreddit](https://libredd.it/) - Private Reddit front-end written in Rust. Faster, private, no JavaScript and no ads.

[Bibliogram](https://bibliogram.art/) - Watch Instagram's public profile in a friendlier page that loads faster, gives downloadable images, eliminates ads and doesn't urge you to sign up.


## Browsers

[Firefox](https://www.mozilla.org/en-US/firefox/new/) - Firefox is the only real alternative to Chrome, as most other browsers are based on Chromium. The interface is very [customizable](https://www.howtogeek.com/334716/how-to-customize-firefoxs-user-interface-with-userchrome.css/), and after the major overhaul with the release of Firefox Quantum, it is extremely fast, secure and private. However, I recommend you to do these [modifications and tweaks](https://www.privacytools.io/browsers/#about_config) that will give you even more privacy and security.

[Librewolf](https://librewolf-community.gitlab.io/) - A fork of Firefox, focused on privacy, security and freedom. It's basically Firefox, but with all the tweaks mentioned before.

[Tor browser](https://www.torproject.org/download/) - The best choice if you need an extra layer of anonymity. It's a modified version of Firefox that will help you to protect yourself against tracking, surveillance, and censorship.


## Search Engine

[Duckduckgo](https://duckduckgo.com/) - The most famous privacy-friendly search engine. The best option if you want to avoid Google.

[Startpage](https://www.startpage.com/) - Another privacy search engine that claims to not track any of your data. The best option if you need better results as it allows users to obtain Google Search results while protecting users' privacy. They were recently acquired which caused some concerns, but they were addressed [here](https://support.startpage.com/index.php?/Knowledgebase/Article/View/1275/0/what-is-startpages-relationship-with-privacy-onesystem1-and-what-does-this-mean-for-my-privacy-protections) and [here](https://github.com/tycrek/degoogle/issues/99#issuecomment-616224650) in my opinion.


## Extensions

[uBlock Origin](https://github.com/gorhill/uBlock#ublock-origin) - An efficient wide-spectrum blocker that is easy on memory, and yet can load and enforce thousands more filters than other popular blockers out there. The basic mode is already good for blocking ads, but there are some [advanced options](https://yewtu.be/watch?v=2lisQQmWQkY) that allows you to even more network requests.

[Privacy redirect](https://github.com/SimonBrazell/privacy-redirect) - Redirects Twitter, YouTube, Instagram, Google Maps, Reddit, Google Search, & Google Translate requests to privacy friendly alternatives, like Nitter, Invidious, FreeTube, Bibliogram, OpenStreetMap, SimplyTranslate & Private Search Engines like DuckDuckGo and Startpage.

[xBrowserSync](https://www.xbrowsersync.org/) - Browser syncing as it should be: secure, anonymous and free! Data is encrypted and decrypted on the device, no one but you can read it. No registration is needed, just enter a randomly generated id or QR code on all devices.

[ClearURLs](https://github.com/ClearURLs/Addon) - Automatically remove tracking elements from URLs to help protect your privacy when browsing through the Internet. 

[Temporary Containers](https://github.com/stoically/temporary-containers) (*Firefox Only*) - Open tabs, websites, and links in automatically managed disposable containers. Containers isolate data websites store (cookies, storage, and more) from each other, enhancing your privacy and security while you browse.

[Cookie AutoDelete](https://github.com/Cookie-AutoDelete/Cookie-AutoDelete) - Automatically removes cookies, lingering sessions, and other information that can be used to spy on you when they are no longer used by open browser tabs. Prevent tracking by other cookies and add only the ones you trust. Easily import and export your cookie whitelist.

[CanvasBlocker](https://github.com/kkapsner/CanvasBlocker) - Allows users to prevent websites from using some Javascript APIs to fingerprint them. Users can choose to block the APIs entirely on some or all websites (which may break some websites) or just block or fake its fingerprinting-friendly readout API. 

[LocalCDN](https://codeberg.org/nobody/LocalCDN) - Emulates Content Delivery Networks locally by intercepting requests, finding the required resource, and injecting it into the environment. This all happens instantaneously, automatically, and no prior configuration is required. 


## Password Manager

[Bitwarden](https://bitwarden.com/) - It's a free and open-source password manager. Bitwarden is among the easiest and safest solutions to store all of your logins and passwords while conveniently keeping them synced between all of your devices and browsers with the [extension](https://github.com/bitwarden/browser). 


## Chat

[Signal](https://signal.org/en/) - Open-source secure messaging with a extreme focus on privacy thanks to the strong encryption by design. The app provides instant messaging, as well as voice and video calling. Its protocol has also been independently audited [(PDF)](https://eprint.iacr.org/2016/1013.pdf).

[Element](https://element.io/) - Open-source privacy-focused chat service with end-to-end encryption. They offer webapps, desktop apps, iOS, and Android (Play Store and F-Droid). Uses the "[Matrix](https://matrix.org/)" protocol for decentralized communication. 

*Signal is an alternative for applications such as Whatsapp, while Element is more similar to Discord.*


## Notes

[Standard Notes](https://standardnotes.org/) - A great alternative for a note-taking service. It is secure, encrypted, and free with apps for Windows, Mac, Linux, iOS, and Android (web-based also available). It features end-to-end encryption on every platform, and a powerful desktop experience with themes and custom editors. It has also been independently audited [(PDF)](https://s3.amazonaws.com/standard-notes/security/Report-SN-Audit.pdf). 

[Joplin](https://joplinapp.org/) - Another great option that is open source and works on any device. It can import Evernote .enex files if you use that. It can also sync with Nextcloud (mentioned above) or any other cloud service. Joplin is a fully-featured note-taking and to-do application which can handle a large number of markdown notes organized into notebooks and tags. 

*I recommend using Standard Notes for small notes, such as a shopping list, while Joplin is more suitable for taking larger notes, such as lecture notes because it supports Markdown, whereas Standard Notes only has Markdown support at the paid tier.*


## Documents

[CryptPad](https://cryptpad.fr/) – CryptPad is a private-by-design alternative to popular office tools and cloud services. It's an open-source "zero knowledge" **collaborative** cloud editor, meaning that you can share your documents and collaborate just like in Google Documents. They offer Rich Text, Code, Presentation, Sheet (beta), Poll, Kanban, Whiteboard, and CryptDrive.

[crypt.ee](https://crypt.ee/) - This is a privacy-focused platform for photo and document storage and editing. They don't even require an email address to sign up. It's faster than CryptPad but you can't share your documents with anyone or collaborate.

[LibreOffice](https://www.libreoffice.org) – LibreOffice is a powerful office suite - its clean interface and feature-rich tools help you unleash your creativity and enhance your productivity. LibreOffice includes several applications that make it the most powerful Free and Open Source office suite on the market - the best alternative to Microsoft Office.


## Synchronization
[Syncthing](https://syncthing.net/) - An open-source continuous file synchronization program. It synchronizes files between two or more computers in real time, safely protected from prying eyes. Your data is your data alone and you deserve to choose where it is stored, whether it is shared with some third party, and how it's transmitted over the internet. Syncthing replaces proprietary sync and cloud services with something open, trustworthy and decentralized.

[Mega](https://mega.io/) / [Nextcloud](https://nextcloud.com/signup/) - Nextcloud is usually self-hosted, but you can choose to host it with a provider that offers a free tier. In this case, both Nextcloud and Mega are typical cloud storage services like Google Drive. While they are more reliable than Google Drive, OneDrive or Dropbox, I would recommend encrypting sensitive files with [Cryptomator](https://cryptomator.org/) before uploading them to their servers.


## File sharing

[Send](https://send.vis.ee/) - A fork of Mozilla's Firefox Send. Mozilla discontinued Send, this fork is a community effort to keep the project up-to-date and alive. Send lets you share files with end-to-end encryption and a link that automatically expires. So you can keep what you share private and make sure your stuff doesn’t stay online forever.

[OnionShare](https://onionshare.org/) If you need an extra layer of privacy, this is a great option, it lets you securely and anonymously share a file of any size.  A web server is started, making OnionShare accessible over the Internet. An unguessable address is generated and is shared for the recipient to open in the Tor Browser to download the files. The huge drawback of this software is that it is very slow, as the transfer is made through the Tor Network.



## File encryption

[7-Zip](https://7-zip.org/) is a free and open-source file archiver, a utility used to place groups of files within compressed containers known as "archives". On Linux, MacOS etc. the command-line tool p7zip is used and integrates into various interfaces such as FileRoller, Xarchiver, Ark. 

[Cryptomator](https://cryptomator.org/) - It encrypts your data quickly and easily by creating a virtual hard drive called vault, everything you put onto the virtual hard drive ends up encrypted. Afterwards you upload them protected to your favorite cloud service. Open source software: No backdoors, no registration. 


## Email

[Protonmail](https://protonmail.com/) - One of the top privacy-focused email providers. It's easy to use, anonymous and has end-to-end encryption. Servers are in Switzerland in an underground guarded bunker that they claim can "survive a nuclear attack". Free accounts up to 500 MB.

[Tutanota](https://tutanota.com/) - Another good free email alternative, Tutanota is an email service with a focus on security and privacy through the use of encryption, located in Germany. Free accounts up to 1 GB.

If you want some aliases for your email you can choose of these two services:

[Anonaddy](https://anonaddy.com/) - Anonymous forwarding email service. Forwarded email can be encrypted (OpenPGP). Create Unlimited Email Aliases For Free

[Simplelogin](https://simplelogin.io/) - Another anonymous forwarding email service.

*The main difference between the free tiers is that Anonaddy offers an unlimited number of aliases, while SimpleLogin offers 15. However, with SimpleLogin you can reply with your aliases, while with Anonaddy's free level you cannot.*

## Calendar and contacts

[Nextcloud](https://nextcloud.com/signup/) - Nextcloud also offers a calendar solution using CalDAV and CardDAV. 

*Another option would be using an email provider, most email providers also offer calendar and or contacts sync services. I would recommend using Protonmail or Tutanota.*

## VPN

[ProtonVPN](https://protonvpn.com/) - Strong contender in the VPN space, and they have been around since 2016. ProtonVPN is based in Switzerland and is the only good option if you're looking for a free VPN, has no ads and no speed limits. It has also been [independently audited](https://protonvpn.com/blog/open-source/).

## Media Player
[mpv](https://mpv.io/) - Simple, extensible, customizable and cross-platform.

[VLC](https://www.videolan.org/vlc/) - Feature-rich, powerful and runs on all platforms.


### If you are looking for an alternative to one of the apps you are currently using and it's not listed here, you should look at [AlternativeTo](https://alternativeto.net/).