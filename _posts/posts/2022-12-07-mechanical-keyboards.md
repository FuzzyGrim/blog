---
layout: post
title: "Going down the mechanical keyboard rabbit hole"
date: 2022-12-07
excerpt: 'The "endgame" is a lie.'
tags: [keyboards]
---

The first keyboard I bought was the [Akko 3084](https://www.amazon.es/gp/product/B0899YGQ7S), mainly because I liked the way it looked.

<img src="https://user-images.githubusercontent.com/34800654/206147256-2f5b7352-935c-4ef2-8974-703ae1595f56.jpg" style="width:30em; max-height:100%">

My initial impressions were good, however, after a while of use, I noticed that some of the keys were making a pinging sound that started to get more annoying every day. So, as usual, I searched for how to fix this problem and found [r/MechanicalKeyboards](https://old.reddit.com/r/MechanicalKeyboards/), a subreddit dedicated to this hobby. The main solutions provided by users were to disassemble the keyboard and add foam or desolder the switches and lubricate the springs. However, the Akko could not be [disassembled](https://www.youtube.com/watch?v=v437MTktIpg) and isn't hotswap. So, this time I decided to inquire about keyboards and get a better one. This post is a summary of what I learned during the process, the keyboard I ended up buying and my impressions.

# What I looked for

As I started getting hooked into the hobby, it became clear to me that I needed a hotswap keyboard. Most keyboards aren't perfect out of the box and require modifications, so I wanted to get a keyboard that could be easily disassembled and modded. With a budget of around 150€, I began looking at the many options for keyboard components, including the PCB, switches, keycaps, stabilizers, and case. As choosing between the many of options for each component started to become overwhelming, to simplify the process, I decided to purchase a keyboard kit that included the bare essentials and I just needed to add switches and keycaps on it.

## Keyboard kit
<figure class="half">
    <img style="margin-right: .2em" src="https://www.ashkeebs.com/wp-content/uploads/2022/03/blkchr-45%E5%B8%A6%E7%A1%85%E8%83%B6_OctaneCamera_a.jpg">
    <img src="https://www.ashkeebs.com/wp-content/uploads/2022/03/qk65-alu-plt.jpg">
    <figcaption style="text-align: center;">QK65</figcaption>
</figure>

The most important factor when deciding on a keyboard kit is the [form factor](https://thegamingsetup.com/wp-content/uploads/Keyboard-Size-Guide-The-Gaming-Setup-scaled.webp), which is the physical shape and number of keys on the keyboard. The most common form factors are 60%, 65%, 75%, TKL and 100%. Basically it is a compromise between compactness and number of keys, however, keep in mind that even if you don't have some keys, you can always use some shortcut to access them. My previous keyboard had a 75% layout, which is a compact keyboard without a numeric keypad. It was a great layout, but since I don't often use the F keys, I would like to buy a 65% keyboard and, on the rare occasions when I needed to use them, I could press the FN key with the number of the function. I didn't want to go down to a 60% keyboard because I wanted to have arrow keys.

Another important factor is the keyboard layout, with ANSI and ISO being the two most popular: 

<figure class="half">
    <img style="margin-right: .2em" src="https://deskthority.net/wiki/images/thumb/5/54/ANSI_layout_basic.svg/616px-ANSI_layout_basic.svg.png">
    <img src="https://deskthority.net/wiki/images/thumb/b/b7/ISO_layout_basic.svg/616px-ISO_layout_basic.svg.png">
    <figcaption style="text-align: center;">Left: ANSI / Right: ISO</figcaption>
</figure>

The main differences are the position and size of the Enter, Backslash and Left Shift keys. Personally, I prefer ANSI because I am used to it and, although I have not used any ISO keyboard as a daily driver, whenever I have used one, I have found the Return and Left Shift keys a bit far from the home row. However, some Europeans prefer ISO because it allows you to have access to some language specific characters, although in my case, I can access all Spanish keys with ANSI using the `altgr-intl` variant with the `us` layout on my linux machine, which can be done by running `setxkbmap -layout us -variant altgr-intl`. Lastly, note that most keycaps are made for ANSI, so if you want ISO, you may have trouble finding the ISO variant of the keycaps you want.

Although not mandatory, I would also like it to have support for [QMK](https://docs.qmk.fm/). QMK is an open source keyboard firmware that allows you to customize the keyboard to your liking. You can make a key do something different when you [press it and when you hold it down](https://docs.qmk.fm/#/mod_tap), different actions depending on [how many times you press a key](https://docs.qmk.fm/#/feature_tap_dance), have some [macros](https://docs.qmk.fm/#/feature_macros), [layers](https://docs.qmk.fm/#/feature_layers), a [leader key](https://docs.qmk.fm/#/feature_leader_key) and many more.

The last thing I looked for was the color of the case, I wanted a black case because it's a great neutral color that matches almost any set of keys and matches the rest of my desktop setup. There are other things to consider but they didn't really matter to me, like bluetooth support, RGB, knobs, etc. By the way, if you want a keyboard with RGB, you'll want to check to see if the switches will be facing north or south.

<figure class="half">
    <img src="https://imgs.search.brave.com/sE48VkIqXgISD_fLglgn8iESgeiPLgwe4QaRn8d55wA/rs:fit:1200:1200:1/g:ce/aHR0cHM6Ly90aGV0/ZWNoZnJvbnRpZXJu/ZXQuZmlsZXMud29y/ZHByZXNzLmNvbS8y/MDIxLzEyL2ltZ180/OTI4LmpwZw">
    <img src="https://imgs.search.brave.com/Rbmcu2j5wjgyomi8_nBhTBF5rSeQQAyrqq3EQPQw184/rs:fit:1200:1200:1/g:ce/aHR0cHM6Ly90aGV0/ZWNoZnJvbnRpZXJu/ZXQuZmlsZXMud29y/ZHByZXNzLmNvbS8y/MDIxLzEyL2ltZ180/OTMxLmpwZw">
</figure>

With north facing switches, most switches will have interference with Cherry profile keycaps. However, south facing switches will have much less of the RGB shining through the keycaps. So this is something to consider that depends on the switches and keycaps you want.

## Switches

![switches](https://user-images.githubusercontent.com/34800654/205942610-47470896-a002-422f-badd-0e7a8baac614.png)

The switches will determine how the keyboard feels and will have a big impact on the sound. They can be divided into three main categories: linear, tactile and clicky. If you have a gaming keyboard, you probably have a linear switch, they are smooth and usually require less force to press. Tactile switches provide feedback when the key is pressed, they are usually a little harder to press and have a bump when pressed. Clicky switches are the loudest, have a bump and produce a sharp click when pressed. Choosing switches will probably be the hardest part, because there are many options and things to consider, such as actuation force, pre-travel, total travel, tactile force, sound, etc. The best way to choose your switches is to test them, either by going to a physical store or buying a [switch tester](https://www.amazon.com/s?k=switch+tester), otherwise, you should read reviews which is what I did. You can find the main data about most switches at [switches.mx](https://switches.mx/) and I would recommend [ThereminGoat's switch reviews](https://github.com/ThereminGoat/switch-scores) to find out which switches are generally considered good.

From what I have tested and learned, these are some great switches to consider, although there are many more:

- Linear switch: Gateron Oil King, Novelkeys Cream, C³ Tangerine and Alpacas
- Silent Lienar switch: Gazzew Bobagum Silent Linear and Silent Alpacas
- Budget Linear switch: Gateron Milky Yellow Pro
- Tactile: Gateron Boba U4T, SP Star Magic Girls and Neapolitan Ice Creams
- Silent Tactile: Gateron Boba U4
- Budget Tactile: Akko Lavender Purple
- Clicky: Kailh Box Jade, Kailh Box Navy and Kailth Box Whites

Most switches usually need some tuning to make them smoother and better sounding. Some of the things you can do are to lube them with [grease](https://www.eloquentclicks.com/product/krytox-grease-gpl-205g0-5ml-9gr/), add [films](https://www.eloquentclicks.com/product/durock-switch-films/) and change the [springs](https://www.eloquentclicks.com/product/tx-springs-60g/). If you want to lubricate your switches, be aware that it is a tedious and time-consuming process, it can take you more than 4 hours if it is your first time. So, if you don't want to bother trying to lube it, there are some switches that come pre-lubed like the Gateron Milky Yellow Pro.

## Keycaps

The keycaps will define the main look of your keyboard, so the most important factor in choosing keys will be whether you like the way they look. If you like RGB, you can opt for [transparent keys](https://es.aliexpress.com/wholesale?catId=0&SearchText=transparent+keys) which will let all the RGB shine through the keys. If you want a more subtle look, you can choose the classic [Black on white](https://www.eloquentclicks.com/product/keychron-black-on-white-doubleshot-pbt-keycap-set/) or [White on black](https://www.eloquentclicks.com/product/keychron-white-on-black-doubleshot-pbt-keycap-set/) keycaps. Or maybe you want to confuse anyone who sees your keyboard with some [Dots](https://candykeys.com/group-buys/gmk-dots) on your keys. You can even opt for [Blank](https://keycapsss.com/keyboard-parts/keycaps/151/np-pbt-blank-keycap-set-iso/ansi?c=16) keycaps if that's your thing.

<figure>
    <img src="https://user-images.githubusercontent.com/34800654/205949731-952dcc8c-2ceb-4394-87b4-ca67f972723a.png" style="width:30em; max-height:100%">
    <figcaption style="text-align: center;">DSA Milkshake Weirdo</figcaption>
</figure>

Another thing to consider is the [keycap profile](https://i1.wp.com/thekeeblog.com/wp-content/uploads/2020/10/gtderEvan.png?w=640&ssl=1), that is, the shape of the keys, which can change the feel and sound of the keyboard. Most prebuild keyboards from famous brands like Razer use the [OEM](https://deskthority.net/wiki/Keyboard_profile#OEM_profile) profile, however, when looking for keycaps on the market, most of them will be [Cherry](https://deskthority.net/wiki/Keyboard_profile#Cherry) profile. Since it is quite difficult to know which one I will like, I will probably choose based solely on the looks of the keys.

Lastly, you should also look at what material the keycaps are made of. They are usually made of PBT or ABS, ABS is much easier to work with and is usually the cheaper, however, they are more prone to shine and are not as durable as PBT. PBT keycaps are more expensive but more durable, however, some of the more expensive keycaps will use high quality ABS because it is easier to have better looking legends and colors.

## What did I end up buying?

When I searched for "65% budget keyboards", most of the recommendations were the QK65. However, it seemed that what I called "budget keyboard" was not the same as what most mechanical keyboard enthusiasts were referring to. The QK65 was at least €160 excluding taxes and it was only the kit, no keycaps or switch included, also I quickly found out that most of the recommendations such as the QK65 was run as something known as a "Group Buy", which basically means it was sold as a pre-order and only for a limited time.

After searching for a while, I found a Spanish store called [Eloquent Clicks](https://www.eloquentclicks.com/) that had one of the 65% keyboard kits that was also recommended, the [KBD67](https://www.eloquentclicks.com/product/kbd67-lite-r4-diy-kit-mechanical-keyboard/) at 115€ (tax included). As the kit was already close to my budget, I decided to add to the cart some [Gateron Yellow Pro](https://www.eloquentclicks.com/product/gateron-yellow-pro-switches/) from the same store and then I could complete my build with some Olivia keycaps, which look like this:

<img src="https://user-images.githubusercontent.com/34800654/205920403-c6e2c58f-adef-4a7a-8940-a512309cff4b.jpg" style="width:40em; max-height:100%">

I know, they look gorgeous with a black case, don't they? Unfortunately, they were originally sold as a group buy and at $140, so yes, I had to settle for some [clones](https://es.aliexpress.com/item/1005002081209745.html) from Aliexpress. With this, the whole keyboard would cost about $175, an amount I was willing to pay for a keyboard I would use for many hours a day. But just when I thought I had decided on my future keyboard... I changed my mind. I discovered a layout that I had never seen before and that was not mentioned in any of the guides. It seems to have grown in popularity recently and ever since I saw a build with it on reddit, I knew it was the layout I wanted for my keyboard, so ladies and gentlemen, I present to you the Alice layout:

<img src="https://user-images.githubusercontent.com/34800654/205975187-38677df1-9b35-46c0-85fb-314b16cf4409.jpg" style="width:40em; max-height:100%">

So I started looking for keyboard kits with the Alice layout and found the only budget keyboard in stock with it at the time, the [Akko Alice Plus](https://en.akkogear.com/product/acr-pro-alice-plus-mechanical-keyboard), although there seem to be other alternatives now such as the [Feker Alice](https://es.aliexpress.com/item/1005004705714166.html). It has the same keys as a 65% layout which is just what I wanted. Akko offers the barebones kit at 119€ and the pre-assembled version at 154€ on their European site, for the pre-assembled version they offered a white construction with the switch options of CS Crystal or CS Silver. From what I read, the Crystal switches were too light for most people and the CS Silver was preferred, however, for some reason Akko only offered the CS Crystal in the black version. So the most logical reason was to get the barebones version with the black case.

But then, I had a fantastic idea, the prebuild black case version came with some black keycaps with pink accents that I could mix with my previous [keyboard keycaps](https://www.amazon.es/gp/product/B0899YGQ7S), which coincidentally were also from Akko and used the same ASA profile, to make some high quality Olivia keycaps instead of having to get ones from Aliexpress. Basically it meant that I would be paying 35€ more than the prebuild version, but I would be getting some Olivia keycaps out of it, and the AKKO keycaps included are usually sold separately at 60€. I finally bought it at 140€ thanks to a discount code. Once they arrived, I changed the alpha keycaps and this is how it ended up looking:

<figure>
    <img src="https://user-images.githubusercontent.com/34800654/205974916-e16e5388-033e-4edb-a0c0-e90af486a461.JPG"  style="width:40em; max-height:100%">
    <figcaption style="text-align: center;">I need another white "B", but otherwise it looks pretty good to me.</figcaption>
</figure>

## My first impressions

I was pleasantly surprised at first, the stabilizers, which are usually the weak point on most prebuilds, were great on this board and didn't need any tweaking. One thing I liked about my previous board was the keycaps and this time around, Akko didn't disappoint either. The keys are made of PBT and are quite thick, with a nice smooth texture. The keys are also nicely printed, the legends are clear and the colors vibrant. It's much better than my previous board, no ping problems and the layout is great and seems much more ergonomic to me. But as I expected, the switches seemed a bit light to me coming from MX Browns, but I liked how smooth they were so I decided to go for another linear switches, this time, some [Oil Kings](https://www.eloquentclicks.com/product/gateron-oil-king/) at 40€ which are heavier and sounded better, and best of all, you don't need to bother lubing them.

## Modding

Although it was perfectly usable as it was, and the sound produced by the keyboard was good to me, I read people recommending some modifications to make this specific board less hollow and less muted. Although I wasn't quite sure what they meant by hollowness, I decided to try some of the mods since it was easy and inexpensive. The two mods I tried were known as the tape mod and the polyfill mod. The tape mod consisted of putting a few layers of tape on the back of the PCB, making the board less muted, while the other mod consisted of filling the case with some polyfill to make it less "hollow". It was after trying these modifications that I realized what they meant by "hollow", as the keyboard sounded much more resonant.

You can hear how it sounds in this [Mega link](https://mega.nz/file/gFsAjA4R#pN57ODMnY-ib8IxvTdsezpwu7_KMQ3MkyNS3xqfwYNc), I started by pressing the non-alpha keys ending with the space bar, then I start typing in a monkeytype test and finally, I go "dfjnandjkcfadnskjcnasjk".

Another thing I tried "modding" was remapping some of the keys. My leader key, the "WIN" key, was too far away from my thumb and I needed for all my window manager shortcuts. So I tried swapping the "LALT" with the "WIN" key with the software provided by Akko but for some reason, it didn't let you change the "WIN" key, ahhh well, I guess that's why people love QMK. While searching for alternatives, I found [keyd](https://github.com/rvaiya/keyd), a general software for remapping keys, and damn if it's amazing and powerful. This is my current configuration:

Another thing I tried "modding" was remapping some of the keys. My leader key, the "WIN" key, was too far away from my thumb and I needed it for all my window manager shortcuts. So I tried to change the "LALT" key with the "WIN" key with the software provided by Akko but for some reason it doesn't let you change the "WIN" key, ahhh I guess that's why people love QMK. While looking for alternatives, I found [keyd](https://github.com/rvaiya/keyd), a general key remapping software, and it's awesome and powerful. This is my current configuration:

```
[ids]
3151:4003

[main]

leftalt = layer(meta)
leftmeta = timeout(overload(alt, macro(\(\) left)), 120, overload(alt, noop))

capslock = timeout(esc, 300, overload(capslock, noop))

esc = `

[capslock:C]
h = left
j = down
k = up
l = right
space = swap(numpad)

[numpad:C]
u = 7
i = 8
o = 9
j = 4
k = 5
l = 6
n = 0
m = 1
, = 2
. = 3

[capslock+shift]
h = C-left
j = C-down
k = C-up
l = C-right
```

Let's break it down:

```
[ids]
3151:4003
```

This is the device id of my keyboard, you can find it with the `sudo keyd -m` command. This is necessary to make sure that keyd only remaps the keys of this specific keyboard and not any other device you may have connected.

```
leftalt = layer(meta)
leftmeta = timeout(overload(alt, macro(\(\) left)), 120, overload(alt, noop))
```

Here I am remapping the left alt key to the meta key which is the "Win" key in my case. I am also remapping the left meta key to be a timeout key, which means that if you tap the key for less than 120ms, it will type () and if you hold it it will act like the alt key. This also makes it so that if you change your mind after pressing the left meta key, you can cancel the () by holding it down for more than 120ms.

```
capslock = timeout(overload(capslock, esc), 300, overload(capslock, noop))
```

Here I am remapping the capslock key to be also a timeout key, if you tap the key it will send an escape key but if you hold it down for more than 300ms, it will activate the layer called "capslock". This is useful for me as I don't need the capslock key and the esc key is quite far away, and I can cancel the esc it if I need to.

```
esc = `
```

As I can already use the esc key with the capslock key, I can remap the esc key to be the key below the esc key, which is the backtick key. It is a key that is missing from the 65% design and I use it quite often when writing blocks of code in Markdown.

```
[capslock:C]
h = left
j = down
k = up
l = right
space = swap(numpad)
```

Here I define the layer called "capslock", which is activated when you hold down the capslock key for more than 300ms. Basically it acts as if you hold down the "Control" key except when you press the "hjkl" keys, in which case it will act as arrow keys. Also, when you press the space key, it will switch to the layer called "numpad". Although I originally said I wanted a 65% keyboard because I needed the arrow keys, thanks to this software I could perfectly go to a 60% layout.

```
[numpad:C]
u = 7
i = 8
o = 9
j = 4
k = 5
l = 6
n = 0
m = 1
, = 2
. = 3
```

The layer numpad also acts as "Control" but when you press the "uiojklmn,." keys, it will write a number acting as a numpad. Although I rarely need a numpad, it's nice to have something for the occasional times that I might need it.

```
[capslock+shift]
h = C-left
j = C-down
k = C-up
l = C-right
```

There are times when I also want to use the "Control" key with the arrow keys, so with this configuration I can use the "capslock+shift" combination to act as "Control" with the arrow keys, with this I will never need to move my fingers from the home row to the arrow keys.

## What's next?

I'm really happy with my keyboard, it's a great keyboard for the price and I love the sound of it, I now just enjoy typing. It was a really fun experience to learn about the world of mechanical keyboards and I'm happy to find another interesting hobby. With this keyboard I can easily try out other switches, such as the Boba U4T which I've heard great things about, but after discovering the "Alice" layout, I have delved down another rabbit hole within the mechanical keyboard hobby, the ergo keyboard rabbit hole. Sadly and and not surprisingly, this means that I already have some plans for my next keyboard and the current one will not be my "endgame".

<img src="https://user-images.githubusercontent.com/34800654/206031760-5e746c43-1817-4281-9b55-62f99db7bb07.jpg" style="width:35em; max-height:100%">