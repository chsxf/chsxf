---
layout: post
title: "The Untitled Project - Why Apple Only?"
date: 2022-03-19 12:00:00 +0100
image: /assets/posts/3/apple-ecosystem.png
description: >
  In my previous post, I introduced The Untitled Project as a project targeting only Apple platforms. This may sound strange. Obviously, using only Apple proprietary frameworks basically prevents you from releasing your apps and games anywhere else easily. Why would anyone make that choice in the first place? In this post, I'll talk about my reasons, other than personal preference.
---

In my [previous post](/2022/01/15/2-the-untitled-project.html), I introduced **The Untitled Project** as a project targeting only Apple platforms.

This may sound strange. Obviously, using only Apple proprietary frameworks basically prevents you from releasing your apps and games anywhere else easily. Why would anyone make that choice in the first place? In this post, I'll talk about my reasons, other than personal preference.

# The Platforms

![The Apple Ecosystem](/assets/posts/3/apple-ecosystem.png)
_The Apple Ecosystem_

If you're not familiar with the Apple ecosystem, three major platforms are available to you as an app developer: iOS (+ iPadOS), macOS and tvOS. There's a fourth one, watchOS, dedicated to the Apple Watch, but the specific form factor and the applicable set of restrictions make it very difficult to design apps the same way.

iOS is obviously the most popular one with more than 1 billion devices in the wild, and about 30% of the mobile market share[^source-mobile] (46% in the tablet space even[^source-tablet]). macOS is far from that with only about 16% of market share[^source-desktop] (but only about 3% of the Steam installed base[^source-steam]). And tvOS is even lower as the Apple TV remains a niche product.

Does that mean there is only one actual viable platform? No, thanks to the tight integration of the Apple ecosystem.

# The Unified Codebase

The three different operating systems actually have a lot in common. Apple engineered their development tools around Frameworks that, for the most part, are universally available on all operating systems. Those who differ are mostly related to specific capabilities (like telephony) or types of interaction (touch vs mouse & keyboard).

In the gaming space though, the main frameworks (GameKit, Game Controller, GameplayKit, SpriteKit, SceneKit...) are generally available with very few notable differences. You generally end up writing the same code for all platforms.

To this regards, targeting Apple platforms makes sense. You don't have to rely on different codebases. You don't have to support different guidelines or backends. Everything works almost the same way. That simplifies development but also testing, marketing, and so on.

# What About a 3rd Party Middle-ware Like Unity?

You would be right thinking that tools like Unity are specifically designed with the same goal in mind: a middle-ware providing an abstraction layer and a unified codebase for your games. That's true only to some extent.

Indeed, Unity will leverage most of the hard work for you on multiple platforms (compiling shaders, compressing textures, etc). And of course it provides an almost complete suite of tools in its editor.

However, as an abstraction layer, it also needs to be generic. Unity will basically act as a common denominator. On mobile, for example, support of iCloud or Game Center is very limited. You have to rely on additional components or native plugins to handle these kind of features extensively. Same for Android.

Also, if your game is available on mobile, PC and consoles, the number of different versions you have to maintain grows rapidly. That may not be a problem for a well established studio, but for a single hobbyist developer, that's a lot to handle.

# The Low Tech Approach

Another topic of concern is the performance overhead of Unity. Don't get me wrong. I work with Unity during my work days. Great games that run perfectly well have been done with Unity. However, as Unity mixes C++ and C#, the engine has to deal with the C# managed memory model. Passing information back and forth between the C++ core of engine to the C# environment where lies the user code can be costly.

A few months ago, I did a [very small experiment](https://twitter.com/chsxf/status/1411710876130938882) to evaluate this cost. Unity handled only 62.5% as much sprites as a native Swift app with SpriteKit. It is of course highly anecdotal, but demonstrates nonetheless that you can be way closer to the [Metal](https://developer.apple.com/metal/), pun intended, without Unity. And thus need less optimization along the way.

Same applies to the actual build size of your games, and therefore the storage space needed to install them. Every game made with Unity (or any other 3rd-party game engine) actually embarks the compiled code of the engine in complement of its own code and assets. Games made with SpriteKit and the other Apple frameworks do not. The framework are loosely linked and loaded directly from the operating system. That's why a build of an empty Unity project weighs about 30 MB. An empty SpriteKit project will be 30 times less.

Both of these aspects are helping bringing your apps and games to low-end hardware more easily, hence reducing the need of replacing still very capable devices.

# The Hardware

Sure, Macs are not gaming machines per say. For a long time, only high-end Macs have been equipped with proper discrete GPUs, leaving only suffering Intel integrated GPUs to the rest of the crowd. That doesn't mean those low-end machines can't run _some_ games.

Sure, playing on a touch device is nothing compared to using a game controller. And even so, there are a lot of mobile players out there. The vast majority is not using a game controller, and if it was ever needed, iOS now offers a [built-in virtual controller](https://developer.apple.com/documentation/gamecontroller/gcvirtualcontroller) game developers can implement in their games to lower the entry barrier.

There is an important demand for quality games on the Mac, on iPhones and iPads, and even on Apple TV. Unfortunately for gamers, at this point, only 1 out of 20 significant games released on PC are available on Mac. Fortunately for game developers, that also means less competition.

# In Conclusion

If you have the ability and work force to port and test on every significant platform you like, from a commercial perspective, going "Apple-only" is an unwise choice. The smartest move is taking the "multiplatform middle-ware" road and release everywhere. It has a cost but, handled carefully, you will probably recoup your investment pretty quickly.

However, for a solo developer, targeting a single platform or ecosystem can also be totally viable. You won't need as much revenue to be sustainable. You will be able to focus more on your actual product. Supporting your customers will also be simpler with less devices to test on (for example, Android has almost 20,000 unique device models, against about 20 on iOS/iPadOS).

As a solo hobbyist developer on this project, I stand by my choice. If this project goes to the end, the game will be exclusively released on macOS, tvOS and iOS/iPadOS.

[^source-mobile]: [StatCounter Global Stats - Mobile Worldwide](https://gs.statcounter.com/os-market-share/mobile/worldwide#monthly-202102-202202)
[^source-tablet]: [StatCounter Global Stats - Tablet Worldwide](https://gs.statcounter.com/os-market-share/tablet/worldwide#monthly-202102-202202)
[^source-desktop]: [StatCounter Global Stats - Desktop Worldwide](https://gs.statcounter.com/os-market-share/desktop/worldwide/#monthly-202102-202202)
[^source-steam]: [Steam Hardware Survey](https://store.steampowered.com/hwsurvey)
