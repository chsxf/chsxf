---
layout: post
title: "The Untitled Project"
date: 2022-01-15 12:00:00 +0100
image: /assets/posts/2/sktestisomap.png
tags:
  - SKTetris
  - SpriteKit
  - The Untitled Project
description: >
  In 2021, I decided to learn Swift, SpriteKit and many of the other gaming-related Apple frameworks. That's how SKTetris was born. And now, I want to do something bigger. But I don't know what. Hence why, in this post, I call it The Untitled Project.
---

When in 2021 I decided to learn Swift, I went right away for the fun stuff. As a game developer, I obviously started making small game experiments and chose [SpriteKit](https://developer.apple.com/spritekit/), Apple's dedicated 2D framework, as my main "engine".

All of this initial forray gave birth to my first finished Swift game, [SKTetris](https://github.com/chsxf/SKTetris), a far from perfect recreation of Tetris. But I learned a lot along the way about the limitations of SpriteKit, how to implement audio, mouse selection or controller support. It also brought me some gifts I wasn't expecting, like [how to notarize](https://developer.apple.com/documentation/security/notarizing_macos_software_before_distribution) an app and ease distribution on macOS.

![SKTetris](/assets/posts/2/sktetris.png)
_SKTetris_

However, despite having been really fun to create, SKTetris remains a small scope project. That's why I chose it in the first place. I needed something so simple and well-documented that I could focus on learning the tools instead of figuring out what the game had to be.

# SpriteKit Mark II

_But now, I want to do something bigger._ Among my recent experiments, I built a very basic isometric map manager. For the moment, there's no fancy editor. All it does is rendering a map from raw data in the right order with as few draw calls as possible. And it works pretty well so far!

![Isometric Map Test in SpriteKit](/assets/posts/2/sktestisomap.png)
_Isometric Map Test in SpriteKit_

That's why I've decided to go for a 2D isometric game. What will it be? I don't know. What genre? No idea yet. I have several game ideas in mind, but it is too early to settle on one. I will start creating some fundational tools and build upon them. That's why, for the time being, I will refer to this game as **The Untitled Project**. The name will come in due time, when the road emerges from the creative fog. The resulting tech stack will probably be separate and reusable and, therefore, have a proper designation too.

The only certainty is that it will use SpriteKit and the other Apple frameworks, and thus will only run on macOS, iOS/iPadOS and tvOS. Maybe there will be a compagnion app for watchOS. Who knows?

So embark with me in this new adventure. Everything, except maybe the assets, will be hosted on [GitHub](https://github.com/chsxf) in public repositories.
