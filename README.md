# Personal Blog

Link to my [website](https://www.fuzzygrim.com), powered by [Jekyll](https://jekyllrb.com/) and hosted with [Cloudflare Pages](https://pages.cloudflare.com/).

This blog is a fork of the [Moon](https://github.com/TaylanTatli/Moon) theme with some notable changes such as:

  - Add post date and reading time in post list ([commit](https://github.com/FuzzyGrim/blog/commit/46494adc9902aa8d10f033cbe896817fbc97700a))
  - Add project category ([commit](https://github.com/FuzzyGrim/blog/commit/32578a4d6ad586eeff253ec37a165f7c95e27565))
  - Add season category ([commit](https://github.com/FuzzyGrim/blog/commit/9797a7307f75d4f1142c658601e9ef054d822afa))
  - Remove fade animations on post list ([commit](https://github.com/FuzzyGrim/blog/commit/c977a6bfb521935f731f80a6b8a09db33ee72341))
  - Make MathJax optional per post ([commit](https://github.com/FuzzyGrim/blog/commit/5a2a051469fa67ed44ed8d80516f943a2f9a0da4))
  - Switch to local fonts ([commit](https://github.com/FuzzyGrim/blog/commit/80c4aff34eeb85fe06f09cab1ac0105fbb764f97), [commit](https://github.com/FuzzyGrim/blog/commit/45d82ba5bea55a65fc351095f2be6cb6cce3b974))
  - Fix Gems related build errors([commit](https://github.com/FuzzyGrim/blog/commit/9b502ff1347a84b0bc67904e33a2be24def8b83d), [commit](https://github.com/FuzzyGrim/blog/commit/7ddbfe493edbaf4455f449966c228c645cee0e8a), [commit](https://github.com/FuzzyGrim/blog/commit/3ed7a3c7192a494ff32809fe6621ec8371fa2ae0))
  - Fix disappearing content when pressing back button in Safari  ([commit](https://github.com/FuzzyGrim/blog/commit/f7cac2167e03e2420efbb0667d3e36bcbc9ac46b))
  - Fix responsiveness of footer in posts ([commit](https://github.com/FuzzyGrim/blog/commit/6e8cd710cc4182ba620ce3fb25311a84d760a6ed), [commit](https://github.com/FuzzyGrim/blog/commit/a6de60db08138b8b520db2c67399c58704d8f3c8))

      
## Installation
* Fork the [repo](https://github.com/FuzzyGrim/blog)
* Edit `_config.yml` file.
* Remove posts, projects and seasons from `_posts` folder and add yours.
* Edit `index.md` file in `about` folder.
* Change repo name to `YourUserName.github.io`
     
That's all.

## Site Setup
A quick checklist of the files you’ll want to edit to get up and running.    

### Site Wide Configuration
`_config.yml` is your friend. Open it up and personalize it. Most variables are self explanatory but here's an explanation of each if needed:

#### title

The title of your site... shocker!

Example `title: My Awesome Site`

#### bio

The description to show on your homepage.

#### description

The description to use for meta tags and navigation menu.

#### reading_time

Set true to show reading time for posts. And set `words_per_minute`, mine is 225.

#### logo
Your site's logo. It will show on homepage and navigation menu. Also used for twitter meta tags.

#### background
Image for background. If you don't set it, color will be used as a background.

#### MathJax
It's enabled. But if you don't want to use it. Set it false in  `_config.yml`.

#### Third Party Services
You can set your username for third party services like matrix, github, etc. in `_config.yml`. If you don't want to use one of them, just remove it or comment it out. If you add `commento-url` or `umami` to `_config.yml`, they will be used for comments and analytics.

---

### Navigation Links

To set what links appear in the top navigation edit `_data/navigation.yml`. Use the following format to set the URL and title for as many links as you'd like. *External links will open in a new window.*

{% highlight yaml %}
- title: Home
  url: /

- title: Blog
  url: /blog/

- title: Projects
  url: /projects/

- title: About
  url: /about/

{% endhighlight %}

---

## Layouts and Content

This theme uses [Jekyll Compress](https://github.com/penibelst/jekyll-compress-html) to compress html output. But it can cause errors if you use "linenos" (line numbers). I suggest don't use line numbers for codes, because it won't look good with this theme, also i didn't give a proper style for them. If you insist to use line numbers, just remove `layout: compress` string from layouts. It will disable compressing.

### Feature Image

You can set feature image per post. Just add `feature: some link` to your post's front matter.

```
feature: /assets/img/some-image.png
or
feaure: http://example.com/some-image.png
```    
 This also will be used for twitter card:

![Twitter Card](https://cloud.githubusercontent.com/assets/754514/14509719/61c5751c-01d6-11e6-8c29-ce8ccad149bf.png)