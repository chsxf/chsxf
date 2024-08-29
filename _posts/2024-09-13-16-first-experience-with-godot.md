---
layout: post
title: "My First Experience With Godot"
image: /assets/posts/16/godot.jpg
date: 2024-09-13
tags:
  - Godot
  - Game Dev
  - GMTK Jam
description: >
  A few weeks ago was held the 2024 edition of the GMTK Game Jam. I participated and released a game. However, this year, I put up a complementary challenge: using the Godot game engine. In this post, I explain how this first experience went.
---

A few weeks ago was held the [2024 edition of the GMTK Game Jam](https://itch.io/jam/gmtk-2024). And, like [previous](https://chsxf.itch.io/top-and-tom) [years](https://chsxf.itch.io/top-and-tom), I participated and released a game: [Web Cycles](https://chsxf.itch.io/web-cycles). However, this year, I put up a complementary challenge: using [Godot](https://godotengine.org/). Here's a small post-mortem of my first eperience with the engine.

![](/assets/posts/16/godot.jpg)

# My Motivations

I've been using Unity since 2013 almost daily, at work and during game jams. Even though I've moved to SpriteKit for my personal projects, it cannot export games to Windows or for the web. WebGL exports are especially important for game jams as it allows people to safely play games with even installing them. So far, Unity was the perfect candidate.

However, in september last year, management at Unity decided to introduce a new way of monetizing the engine: the [Unity Runtime Fee](https://unity.com/fr/products/pricing-updates). It would be an understatement to say that developers around the world were disappointed. The first proposed solution was an insult to every game developer that was using the engine. Luckily, the final draft was much more sensible, but a lot of trust has been lost along the way. And Unity has a long road ahead to regain that, even if the Runtime Fee has finally [been canceled](https://unity.com/fr/blog/unity-is-canceling-the-runtime-fee).

Enters Godot. The same day Unity announced their new fee, Godot introduced its [development fund](https://godotengine.org/article/godot-developer-fund/). Unity's bad move gave it a lot of attention as many indie devs and small to medium studios looked for an alternative to Unity. And so did I!

But it can be pretty hard while working to learn new technologies that are not directly applicable to the production at hand. That's why I decided to give Godot a try during my favorite game jam of the year, to see for myself if there's any potential for me or for the studio in the future.

_(Alt Shift, the studio I'm part of, still uses Unity and will continue in the foreseable future. This post, like all the content of this website, exposes only my personal views)_

# A Bit of Disclaimer

As I said earlier, I've used Unity for more than ten years now and, during this time, exclusively with C#. The fact that Godot embraced C# officially a few years ago makes it a very good alternative choice over Unreal that uses C++. In order to avoid starting completely fresh during the game jam, I decided to follow a few tutorials (notably [from Brackeys](https://www.youtube.com/watch?v=LOhfqjmasi0)) to get familiar with the editor and the components. The tutorial uses GDScript, Godot's first-party scripting language, but I converted most code pretty easily to C#.

{% include youtube.html video_id="LOhfqjmasi0" %}

Unfortunately, I discovered a few days before the game jam that the WebGL export template for Godot wasn't available in the dotnet version of the engine. If I wanted to export for the web, I would be stuck with GDScript. Many people consider GDScript a very good choice for Godot, so I decided to take the bullet and went with it.

So basically, everything that follows is me trying my best to code a game in about 96 hours, while discovering pretty much everything about the engine. I'm certain some of my conclusions are wrong based on that, and that solutions exist that I didn't find by myself.

# The Good Bits

## Light but Mighty

Fortunately, many things are advocating for Godot from the get go. For starters, the editor / engine is very small. About 250 MB on macOS, as a Universal binary which supports x86_64 as well as Apple Silicon architectures. That's impressive compared to the 8+ GB of the Unity Editor 6000. Sure, you have to install the dotnet runtime by yourself if you want to use the corresponding version, but that's a minor issue and does not explain the size difference.

Of course, the editor is in itself way less demanding on the system (putting aside the memory required for your project). This is especially important for me as I sometimes use a Intel MacBook Air 2020 with 8 GB of RAM and it struggles to handle Unity and Rider simultaneously (as Rider is a memory hog). Godot + Visual Studio Code would be a breath of fresh air!

Projects are also way smaller, as there is no Library folder. These special Unity folders, present in every project, can take a lot of space. For example, for my GMTK Jam 2023 project, the overall size of the Unity project folder is about 2 GB, from which the Library folder takes 1.37 GB. And, obviously, the bigger the project, the bigger the Library folder.

The only downsides of the small size of Godot is that you have to download export templates separately (and for all platforms at once). This download weighs about 1 GB.

## Complete Editor

Just like Unity, the Godot editor is mostly fully featured, with views for the scene / world, the hierarchy, an inspector, animation tools, etc. I have not explored the full panel, but expect for coding, I never had to leave the editor while building my game.

![Godot Engine Editor](/assets/posts/16/godot-editor.png)

Godot also includes a debugger, that is perfectible but totally serviceable anyway. However, the main difference with Unity is that the game does not run inside the editor itself. It runs in another instance of the player and the editor / debugger connects to it remotely (hance the Remote view you use to see the current hierarchy, and to inspect objects).

There are also many types of nodes already provided by Godot. Many of my use cases during the game jam have been covered from the get go.

## One Node, One Purpose

In Unity, the scene tree is composed of Game Objects, that then hold multiple components to give them purpose. That works but this is not ideal. It becomes quickly difficult to identify which object holds what components, and we sometimes have to create one Game Object per component (generally global managers) and name them accordingly. That clearly defeats the purpose.

Godot took a drastically different approach. Every object in the scene tree is a node, and each node has one purpose only. Each built-in node has a specific icon in the scene tree that helps identification even more (it is probably possible for custom nodes, but I didn't dig into that yet). Working with objects in the hierarchy has never been easier!

## Call Down, Signal Up

This design pattern is not specific to Godot as this is a best practice in many cases.

Basically, within a hierarchy of objects, if you want to communicate things from a parent to a child node, you just have to call a method on one of the child nodes, because parents generally know how their children are organized. In the other direction, as children have no knowledge of their parent's structure and ancestry, calling methods generally ends up in a beautiful mess.

Enter events. Or signals. Or whatever you call them. Children are supposed to emit a signal or invoke an event to notify one of their parents that something notable has happened. Communication between sibling nodes should also pass through signals or events: one sibling emits a signal, the parent catches the signal, then calls the right method on another child sibling node.

This design can (and should) be implemented in any engine. Unity included. However, it is especially embroiled in Godot's DNA as, by default, you can't see the child nodes of a scene nested inside another. In Unity, all children of a prefab are visible by default if you expand the corresponding hierarchy. That often leads to unfortunate connections between GameObjects and components that should never have been linked. By hiding the child nodes, Godot does not train you to connect them in unwanted fashion. If you decide to engage with a worse practice, you have to want it and act accordingly. Therefore, my Godot project instantly felt more organized than any of my previous Unity projects.

# Not All Roses

Unfortunately, for me at least, everything is not perfect within Godot.

## A Node Path is Not a Node Reference

When using GDScript, Godot will encourage you to use `@onready` variables. They are basically variables that are initialized when the object is ready to be used. These variables are initialized with a reference to another node thanks to a relative path.

```
@onready var my_node: Node = $relative/path/to/my/Node
```

However, as the path to the referenced node is hardcoded into your scripts, they won't be updated if you change the name or position of the referenced node in the hierarchy, or of any of its ancestors. And if you're not careful, that's something that will come to bite you at runtime the next time you run your game. If the problem is in a less frequently tested area, it may be missed. Be careful.

There is no such things in Unity. We have serialized fields in the inspector, but that would be `@export` variables in Godot, for which the problem doesn't seem to occur. They are updated by the editor when moved or renamed. I would recommend to avoid `@onready` variables when possible, especially on complex trees.

## A Resourceful Mess

Storing structured data with Unity is simple. Create a class deriving from ScriptableObject, then create an asset based on it, and you're almost good to go.

Godot has a similar mechanism called Resources. You create a class deriving from Resource and you can then create an asset based on it. However, in my brief experience, this was way more complicated and unstable than with Unity. I had to reboot the editor multiple times to see my changes reflected in the structure of my resources. That may be due to the fact that I was using Rider as an external text editor, but that was very frustrating. In the end, to save some time, I just used JSON files and a simple function to parse them. But I lost the ability to edit them in the inspector.

## Lost Time

Speaking of unstability or inconsistencies, I also encountered several issues while trying to setup an external editor, wether it's Rider or Visual Studio Code. Procedures are well described for the two IDEs, but in both cases, each time I edited a file, I was granted with a box asking me to reload the file or use the one in memory because it had been edited outside of Godot itself. What is the point of defining an external text editor if you can't reload the files automatically? Turns out there is a totally different setting just for that that is not enabled by default when an external text editor is defined. That should be the case in my opinion.

But, even with that, after having set up Visual Studio Code completely, I was unable to edit my JSON files. Godot would refuse to load them and each time went back to the state it had in memory. I lost half an hour on this strange behaviour, that doesn't seem to occur with Rider. I haven't tried to reproduce it since then, so there was maybe a glitch somewhere, but it was definitely not a smooth ride with external editors. Which is a shame because the built-in editor is missing critical tools, notably for refactoring.

## Where Are My Sprites?

For my project, I used Sprite2D nodes. I was expecting something similar to Unity when you can create sprites within a sprite sheet in advance and use them directly within your Sprite renderer. For the life of me, I wasn't able to find an equivalent in Godot and had to fall back to use a texture region, which was OK but way less convenient, especially when you want to change the sprite of your node at runtime. I had to manually input the regions for each of my sprites in my JSON configuration file to be able to do that. What a waste of time.

## Obscure UI Nodes

The history of UI components within Unity has been a long-running gag among developers. For a long time, we had no built-in UI framework, and then we got Unity UI during the 4.x version cycle. And several years later, Unity UI was deprecated in favor of UI Elements. But, even now, the new framework is not completely ready yet and not backward compatible. So many people remained with the former so far.

Unity UI is a weird framework with its own take on many aspects of the UI. But it's functional and well known, even if it has its drawbacks, especially with layouts. However, there is something that it does very well, and this is anchoring. Thanks to Canvas and RectTransform components, you can basically place any object anywhere you like on the screen and it will adapt to any resolution and aspect ratio.

Godot seems to have a similar approach with its UI nodes. But, the anchoring system seemed much more rudimentary to me. I had a hard time with the UI and ended up compromising what I had in mind to adapt to what I was able to do. I used many Margin Containers to position my objects, but for some things, that didn't sound right. I clearly need more practice with it to see if the problems comes from me or from a lack of functionality in the built-in nodes.

## GDScript is Definitely Not C#

I'm pretty sure it will come as a very unpopular opinion, but I am not a fan of GDScript. To be clear, GDScript seems a very capable language and is tightly integrated with the rest of the editor. But it doesn't align well with my personal preferences.

First, this is a language that uses [significant identation](https://en.wikipedia.org/wiki/Off-side_rule). In other words, it defines blocks using indentation. While it is said to make things more readable, in my experience that's not the case. I'm probably biased after 30 years using C-based languages, but it is really hard for me to identify where blocks end at a glance, especially if you insert blank lines between blocks (I do that a lot to separate groups of closely related instructions).

Second, this is an interpreted language. The Godot editor or the various static analyzers do their job to identify code that won't compile, but there were a lot of occurences in my code where it wasn't possible, mostly when using dictionaries or variants, or when emiting signals. Dictionaries can't be typed, variants can accept every type, and emiting signals seems a place where analyzers should be able to detect parameters types, but they don't. In these cases, you will identify issues only when running the game. I would really love to get typed dictionaries and a way to parse JSON to a known class so that values you get in both cases are typed too. It reminded my of the dark old times of early PHP 5 in many regards.

Finally, the Godot editor does not provide any unit test for GDScript. There are [plugins](https://github.com/bitwes/Gut) for that obviously, but in my opinion, it should be an integral part of the editor flow, just like we have the Test Runner in Unity.

# My Humble Conclusion

Godot is better than Unity, on many levels. The introduction of C# as a first-class citizen changed a lot of things for me when considering an engine. It would make the transition from Unity way easier for us as a studio, as we could bring with us most of the engine-agnostic code we created over the years. As for the type of games we do at Alt Shift, or for the scope of games I do during game jams, it seems a very good match.

However, there are some things that need to be addressed first, some things that we take for granted when working with Godot's competitors.

C# support is still experimental on mobile, even though the actual limitations are not explicitly defined [in the documentation](https://docs.godotengine.org/en/stable/tutorials/scripting/c_sharp/index.html#c-platform-support). Also, WebGL exports are not compatible with C#. That may seem like a minor things, but playing games in the browser has never been so popular, especially in countries with limited access to internet or where app stores are not available. We can't code our games twice for that.

The several issues I got with external editors have to be ironed out. Due to the limitations of the internal editor when it comes to C#, this is really important. Anyone willing to do serious work with Godot would want to work in a dedicated code editor, like VSCode or Rider.

Also, Godot has to provide an easier way to manipulate an important quantity of data. Resources don't work well. Parsing JSON files without validation/mapping is not sustainable.

One thing is now obvious, migrating to Godot from Unity is not easy. Not all your experience will be translated in the process. You'll have to rebuild a significant part of your knowledge base. But that's probably true to every transition between engines. At least, the presence of C# means you won't need to learn a new language, only new APIs.

As for me, I will continue to test Godot during game jams in the years to come. It was fun to use and a breath of fresh air to learn new tools. I recommend everyone to give it a try anyway.
