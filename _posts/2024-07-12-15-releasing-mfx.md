---
layout: post
title: "Releasing MFX"
image: /assets/posts/15/releasing-mfx.png
date: 2024-07-13
tags:
  - PHP
  - MFX
description: >
  A few days ago, I released MFX, a PHP micro-framework I've been working on for the past 10 years. In this post, I will tell you the story of this project and how its 1.0 release has been received.
---

![Example PHP code](/assets/posts/15/releasing-mfx.png)

A few days ago, I released [MFX](https://github.com/chsxf/mfx), a PHP micro-framework I've been working on for the past 10 years. Releasing something to the public can be daunting, but in my humble opinion is necessary to bring your creation to its next stage. And, as you'll see in this post, feedback was very useful.

# Story Time

I've started programming in PHP in 2004, 20+ years ago. Back then, most web hosting providers still used PHP 4, even though PHP 5 was around the corner. PHP 4 had minimalistic object-oriented programming capabilities and no strong typing. PHP 5 was the answer to that.

The new version introduced so many fundational changes that the transition took a long time. However, by the time I changed employer, at the end of 2008, PHP 5 was well established. PHP was then really popular and, moving towards a more strongly typed language, my interest grew as well.

I stayed in this job for 5 years, until my departure in late 2013. The primary focus of the company was browser games. Therefore, I learned a lot of useful lessons about writing high availability websites and APIs with PHP. This was especially true since most of our tools were developed in-house. Errors were made along the way, obviously, but I tried my best, in my position of senior developer, to improve efficiency and reliability along the years. The company ended up with a battle-tested set of tools and a nano-framework as the core of each one of its websites.

After I left, we decided with former colleagues to create our own game studio: Cheese Burgames[^cbg-closure]. We needed some internal back-end tools to support many aspects or our games (localization, game design databases, etc). Of course, I was going to write them in PHP. Nodejs was already a thing, but I could not stand JavaScript (and TypeScript was not mature yet). And so the first stone of MFX was laid.

# Fast Forward to 2016

For three years, Cheese Burgames became my sole occupation. During that time, step by step, MFX has seen a lot of evolution. But, as the sole developer in our team of three, I had to handle the development of our games with Unity in C# as well. And MFX remained an internal project. We never actually used it for any of our clients.

But in 2016, after about a year trying to find a publisher for our game [Revery - Duel of Dreamers](https://chsxf.itch.io/revery-duel-of-dreamers), we had to come to the conclusion that the company wouldn't go for long anymore. And that's when I got in touch with my current employer, [Alt Shift](https://altshift.fr). I was about to became a senior Unity C# developer only, as PHP was not used in the company. That should have been the end of MFX.

However, a few years later, in 2019, we had to build a back-end and an API for one of our clients and I decided to use MFX. It was the opportunity I had missed so far to validate in the field that my design choices were valid. And MFX delivered! We stopped working with this client a year and a half later due to a change in their priorities, but I had my answers.

# The Road to Release

Thanks to that external project, MFX was fine. But it remained an internal tool in essence, even though I started writing some documentation. And after that, we never took a PHP gig with another client. The framework's code was already available on GitHub in 2018 under a MIT license, but it was pretty much unusable without my support.

So I started to slowly extend the documentation. In 2022, I even worked with a former Alt Shift colleague who wanted to use MFX for the website he was building. He has worked with me on the external project involving MFX and was comfortable enough to consider using it on his own products. I took the opportunity to add support for PHP 8, which became mandatory, and for Composer as well. I also fixed some bugs my friend identified along the way.

Over the past year however, the main road blocks remained the missing parts of the documentation. But, writing page after page, I gradually reached completion. It's not perfect. It's probably missing some valuable information. But it's there. Only remained pressing the button.

# MFX 1.0 is Out!

On July 9th, 2024, I [released MFX on GitHub](https://github.com/chsxf/mfx/discussions/25), and published some messages on my socials and on Reddit in the [PHP subreddit](https://www.reddit.com/r/PHP/comments/1dywwl8/mfx_10_is_out/). And boy, I got a lot of feedback. Some [good](https://www.reddit.com/r/PHP/comments/1dywwl8/comment/lcbm29o/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button), some even [very good](https://www.reddit.com/r/PHP/comments/1dywwl8/comment/lcc6ixt/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button). And some [not so much](https://www.reddit.com/r/PHP/comments/1dywwl8/comment/lcdba7v/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button).

But that's the risk I was willing to take. So far, I've almost been the sole developer of MFX. I needed a fresh perspective on it. And I was glad to be criticized this way. In the days that followed, I managed to improve syntax consistency, ease of use, security, and even add Docker support.

# What's Next?

Releasing a first version is one thing, but you need to keep it alive. Fortunately, I am at a stage in CiderKit's development where I will soon need tools to support some aspects of the games. My plan is to recover the source code of the tools I made for Cheese Burgames and update them for the new version of MFX.

Some new features and improvements are also planned for MFX on its own, but the complete list will probably evolve. And of course, I hope some people will try the framework and make some contributions.

[^cbg-closure]: mostly dormant after 2017 and closed since 2020.
