---
layout: post
title: "Managing SKView Render Resolution Through Content Scaling"
date: 2022-10-22 12:00:00 +0100
image: /assets/posts/6/higher-lower-render-resolution.png
description: >
  While working on the authoring tools for The Untitled Project, I realized that running the game at native Retina resolution was not possible on low-end GPUs. I needed a way to render the content of my SKView at a lower resolution. However, the documentation does not provide directions on this subject. In this post, I explain how I finally get to the solution and how to implement it.
---

[SpriteKit](https://developer.apple.com/spritekit/), the Apple proprietary framework for 2D graphics rendering, is a wonderful tool. It is very efficient and makes it easy to create 2D animations or games for Apple devices. However, not being cross-platform mechanically reduces the size of the developer community, unless it is a [considered choice](/2022/03/19/3-the-untitled-project-why-apple-only.html).

# The Problem

While working on the authoring tools for The Untitled Project, I realized that rendering the game at native resolution on an Intel integrated GPU was not possible. Unlike any other device I tested, the game did not run at 60 fps on my Intel MacBook Air 2020 (at 2560 x 1600), despite being equiped with a Core i7 and not thermal throttling. And this scenario repeated with other Intel iGPUs.

![Higher-Lower Render Resolution](/assets/posts/6/higher-lower-render-resolution.png)

My guess was that there were too many pixels to render to get my [deferred lighting model](https://twitter.com/chsxf/status/1523796764020846593) running. So, I looked for a solution to render at a lower resolution. However, I rapidly discovered that my specific use case was not documented in the official documentation. Unfortunately, the small SpriteKit's developer community did not provide a solution either. I went on with the rest of the project and shelved this issue.

# The Key

Fortunately, last week, Apple held the **Ask Apple** developer event through [Slack](https://slack.com) where we could ask any question on a variety of topics, including **Graphics & Games**.

![Ask Apple](/assets/posts/6/ask-apple.png)

I went on with my question about my very specific use case: _How to render a SpriteKit [SKView](https://developer.apple.com/documentation/spritekit/skview) at a lower resolution?_ An Apple engineer quickly responded to me with the solution.

However, why would you want to do that in the first place? As I stated earlier, to improve overall performance on lower-end GPUs. But performance comes in various ways. For pixel art games, if you don't actually need the full native resolution of your device, you could want to render at a lower resolution to avoid thermal throttling or reduce power consumption. Depending on the type of the game, it could be a great life improvement for the players.

# The Takeaway

Now with the solution. Once obtained, it seems somewhat obvious. All I needed to do was to set the content scaling factor of my SKView.

There are two ways of implementing it. In AppKit, `SKView` derives from `NSView`. In any other framework, it derives from `UIView`.

With AppKit then, you need to use [`NSView.scaleUnitSquare(_:)`](https://developer.apple.com/documentation/appkit/nsview/1483721-scaleunitsquare). Or [`UIView.contentScaleFactor`](https://developer.apple.com/documentation/uikit/uiview/1622657-contentscalefactor) anywhere else.

And indeed, the size of the render surface scales accordingly. With a scale of 2, your render surface will see its dimensions divided by 2. You can check that for yourself through a Metal capture while debugging your SpriteKit app.

Anyway, Apple is holding more and more frequent free online events for developers. If you have any question as a developer of an app targeting Apple platforms, sign up to these events and ask your questions. You might be surprised by the answers.
