---
layout: post
title: "My search for a new email provider"
date: 2023-10-23
excerpt: "So many options but so few that meet my requirements"
tags: [privacy, blog]
---

I have decided it’s time to take control over my emails, this means using my own domain, using whatever email client I want and being able to easily switch providers if I want to. Although this has been on my mind for a while, I’ve been putting it off because I didn’t want to deal with the hassle of updating all my accounts with the new email address…

Here is a list of my requirements for an email service:

  - Custom domain support, this is a must and the main reason I’m doing this. The advantage of using a custom domain is that I can easily switch providers without any hassle, as all my online accounts will be linked to my domain and not to the provider.

  - Multiple domains, I want to be able to connect multiple domains to the same mailbox, this way I can have all my emails in one place.

  - IMAP/SMTP support, I don’t want to be locked into a provider’s web or mobile app, I want to be able to use whatever email client I want. With IMAP support, I can also connect my mail to services such as Paperless-ngx for automatic document management.

  - Multiple aliases, I like having one alias for each service I use, this way I can easily remove the alias if I start getting spam. I am not a fan of using catch-all addresses as they can be targeted by spammers with dictionary attacks.

  - Reply from multiple addresses, I want to be able to respond from the same address that the email was sent to. This should be automatic, I don’t want to have to manually create a new identity for each alias.

  - Be able to create folders and rules, I want to organize incoming emails into folders automatically. For example, I want to be able to automatically move all emails from a certain sender to a specific folder. Or move all emails from a certain domain to a specific folder.

  - Need around 2GB of storage, I don’t need much storage as I don’t keep emails for long, and I am not a heavy email user.

  - Good privacy, my threat model is not that high with email, I just don’t want my emails to be scanned for ads or sold to third parties.

  - This is optional, but I would like to have an API or some way to easily create new aliases. For example, Bitwarden has some integration with some services to automatically create new aliases when signing up for a new service. I would like to be able to do something similar with my email provider.

I have been researching different email providers for a while and read many posts and comments on multiple sites and forums, and even tried some of them. I have compiled in this post a list of the most popular ones and my thoughts on them.

## Proton Mail

[Proton Mail](https://proton.me/es-es/mail) is probably the most popular privacy-focused email provider. They offer a perfectly usable free plan with up to 1GB of storage and 150 messages per day. This is the provider I've been using for the last two years since I moved away from Gmail.

Their webmail's UI is great, they also have a mobile app which is OK. The interface is good, but I have had some issues with the app as having infinite loading screens and notifications not working.

They support custom domains in their paid plans, but are a bit expensive. Their cheapest plan is €48 per year and only allows one custom domain, the next plan is €120 per year which comes coupled with a bunch of other services that I'm not interested in.

You can use external mail clients with their paid plans, but you have to use their [bridge](https://proton.me/mail/bridge) which is only available for desktops, no mobile devices. Considering the fair amount of issues I've had with their mobile app, this doesn't seem satisfactory.

Lastly, they only allow 10 email addresses in their €48 per year plan, which is quite limiting if you want to use one alias for each service like I do. For these reasons, I decided to look for other providers.

## Tutanota

I've used [Tutanota](https://tutanota.com/) free plan as a secondary mail for a while, so I have some experience with it. Just like Proton Mail, they are also privacy-focused and offer a usable free plan with 1GB of storage.

For custom domain support you have to pay €36 per year, in this plan they support 3 custom domains which is enough for me currently but could find myself needing more in the future. A big advantage over Proton Mail is that you can use unlimited aliases with custom domains.

Tutanota doesn't have a bridge like Proton Mail, which means you have to use their web or mobile app. Their webmail UI isn't great and was one of the main reasons I moved to Proton Mail. I also had issues before with their iOS app, like not being able to log in and sometimes automatically logging out.

## Fastmail

[Fastmail](https://www.fastmail.com/) is a well known email provider and is quite reputable among tech-savvy users. They offer a reliable service with up to 600 aliases for custom domain in their $50 per year plan, which at the moment considering taxes is around €58 per year, which is quite expensive for me.

They really offer a complete service with a great webmail UI, mobile apps, up to 100 custom domains, IMAP/SMTP support and from what I have seen, they are one of the few providers that supports Apple Mail's push notifications, which is a big plus for me. Not only that, but they even have something like [SimpleLogin](https://simplelogin.io/) built-in, which allows you to have up to [masked emails](https://www.fastmail.help/hc/en-us/articles/4406536368911) for the random services that you don't want to share your domain with.

The main problem I have found is that they are based in Australia, which has a [not so great privacy record](https://www.itnews.com.au/news/fastmail-loses-customers-faces-calls-to-move-over-anti-encryption-laws-519783). However, this should only be a problem if your threat model includes the Australian government, for regular folks who just want to avoid big corporations scanning their emails for ad revenue, this shouldn't be a problem and Fastmail could be a good option to consider if you can afford it.

## mailbox.org

[mailbox.org](https://mailbox.org/en/) is also a well known email provider among privacy-conscious people, they have a long experience in the field and have provided solid email services for many years. Their standard plan is €36 per year, allows multiple custom domains and have IMAP/SMTP support.

The main problem I have found feature-wise is that they only allow 50 aliases in this plan, which is not enough for me and the next plan which allows 250 aliases costs €108 per year, which is expensive for me.

Something strange I have found with their service is that it seems that their SMTP server [doesn't verify the sender email address](https://web.archive.org/web/20210123192856/https://userforum.mailbox.org/topic/mailbox-org-smtp-server-stellt-mails-mit-gefakten-absender-zu). This means that you can send emails from any address, even if you don't own it. This is a bit worrying and a clear issue that they should fix, but they don't seem to acknowledge the problem.

## Posteo

[Posteo](https://posteo.de/en) is another reputable and privacy-oriented service, it's also one of the [cheapest](https://posteo.de/en) options at €12 per year and supports external email clients with IMAP/SMTP.

One thing to note about their service is that, you don't have a spam folder, so you can't manage your own spam. They manage spam centrally, which could be a problem because it would mean that you won't receive any email that they consider spam.

Apart from this, the real dealbreaker for me, is that they don't support custom domains, which is the main reason I'm doing this. It doesn't seem like they have [any plans to support custom domains](https://posteo.de/en/site/faq#owndomains) in the future, so I had to discard them.

## Migadu

[Migadu](https://migadu.com/index.html) has an interesting approach to pricing. Their pricing is based on actual usage, not address space. At only $19 per year, you get a service with IMAP/SMTP support, [almost unlimited custom domains](https://migadu.com/pricing/#what-is-the-domains-limit-on-the-micro-plan) and unlimited mail addresses at a fixed price, and they even offer a [student discount](https://migadu.com/pricing/#do-you-offer-student-discounts)!

The main problem with their $19 plan is that it only allows you to send up to 20 emails per day, which could be quite limiting for some. They don't seem to have a flexible pricing and their next plan, which allows you to send 100 emails per day, costs $90 per year, which is much more expensive.

While I think I can get by with 20 emails a day, since I don't usually send that many emails, the biggest dealbreaker for me were the multiple comments on [Hacker News](https://news.ycombinator.com/item?id=27708580) about many users' bad experience with their service. This includes [short notice on price changes](https://news.ycombinator.com/item?id=27709370), [storage problems](https://news.ycombinator.com/item?id=27709817), [sudden change of your POP3 address](https://news.ycombinator.com/item?id=27709817), [deleted custom rules](https://news.ycombinator.com/item?id=27710623).

## MXroute

Their pricing is based solely on storage space, on their small plan they offer 10GB of storage with unlimited domains and email addresses for $49 per year, and they offer a [Lifetime plan](https://accounts.mxroute.com/index.php?/news/view/53/lifetime-plan/) for $129 which seems like a great deal. Of course, lifetime means the lifetime of MXroute, which who knows for how long they will be around.

They seem to be popular on [LowEndTalk](https://lowendtalk.com/), as they have offered some great deals there in the past, such as [100GB for $40 per year](https://lowendtalk.com/discussion/184324/mxroute-email-hosting-new-large-storage-plans) or a [10GB for $10 per year](https://lowendtalk.com/discussion/182750/mxroute-email-hosting-black-friday-2022) on Black Friday.

They support the basic email protocols such as IMAP/SMTP, and they offer multiple webmail clients such as [Rainloop](https://www.rainloop.net/) and [Roundcube](https://roundcube.net/) and [Crossbox](https://crossbox.io/). Also, this year [they added support for push notifications on iOS](https://headwayapp.co/mxroute-changelog/push-notifications-on-ios-255875), which a big plus as most providers don't support this.

It's recommended only for technical people, as they won't hand-hold you through the process of setting up your email. I have seen comments on how opinionated the owner is, and will sometimes ban incoming mails from certain providers, but overall they seem to be a good option if you are looking for a cheap email provider, especially if you can get one of their deals.

## Purelymail

[Purelymail](https://purelymail.com/) is the cheapest option I have found, and it's the one I ended up choosing. They offer a [simple pricing at $10 per year](https://purelymail.com/pricing) with no hard limits on users, custom domain, storage or anything else. However, they also offer an [advanced pricing](https://purelymail.com/advancedpricing) in which you pay per resource use, which in my case is cheaper than the simple pricing. Using their calculator, I would pay less than $5 per year for my usage.

They pass all my requirements, with an acceptable webmail UI powered by [Roundcube](https://roundcube.net/) but if you don't like it, you can use any external email client as they support IMAP/SMTP. On their website you can manage your account, create new aliases, configure your domains, create routing rules for incoming emails, and manage multiple users.

They even have an [API](https://news.purelymail.com/api/index.html) which allows some basic operations such as creating new aliases, users, domains, etc. This is great as I can create scripts for automating some tasks such as creating new aliases when signing up for a new service.

Now on to the cons, they don't support push notifications on iOS, which is a bit annoying, but I can live with it. Another thing is that their webmail, with the [custom_from](https://github.com/r3c/custom_from) plugin, should set the from address to the address the email was sent to when replying, but it doesn't seem to work, or I haven't been found how to configure it properly. Although I did get it working on Thunderbird, which is what I use most of the time.

More importantly, it's a new service launched in 2019 and is still in beta, although from my usage of a few weeks, I haven't had any issues. And to the main concern, it's currently run entirely by one person, which is a bit worrying if [anything happens to him](https://purelymail.com/docs/companyPolicy#bus). For now, I'm willing to take the risk as I can easily switch providers if anything happens, which was the main reason for doing this in the first place.
