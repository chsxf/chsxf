---
layout: post
title: "Building My Own CSS Parser"
image: /assets/posts/12/Cascading_Style_Sheets.jpg
date: 2023-05-06 12:00:00 +0100
tags:
  - CiderKit
  - CiderCSSKit
description: >
  I've started working on the next chapter of CiderKit: User Interfaces. And that's a big one! In this post I explain why I ended up building my own CSS parser in order to apply styles to my UI elements.
---

Now rendering is at a stable stage (at least stable enough), I've started working on the next chapter of [CiderKit](https://github.com/chsxf/CiderKit): User Interfaces. And that's a big one! User input is can be handled out of the box by [SpriteKit](https://developer.apple.com/spritekit/). However, as far as I know, there is no proper UI toolkit available for SpriteKit. Providing basic UI components like containers, buttons, or even sliders, should be relatively easy. But styling them efficiently can be somewhat challenging. Fortunately, the web solved most of this issue many years ago with Cascading Style Sheets (CSS).

![Cascading Style Sheets](/assets/posts/12/Cascading_Style_Sheets.jpg)

CSS have evolved a lot since their introduction in 1996, going through several monolithic iterations up to version 2.1, then being split in many different independent modules for CSS 3 and 4. From a specification point of view, this is bit of a mess. I only need a subset of it though: no flexbox, no positioning.

# Why?

Because embarking in writing my own CSS parser, I did some research. Unfortunately, to the best of my abilities, I didn't find something that really suited my needs. [SwiftSoup](https://scinfu.github.io/SwiftSoup/) is a full HTML parser, way too complex for my use case. And [others](https://github.com/search?q=swift%20css%20parser&type=repositories) are either not maintained or not Swift packages (and I don't want to invest anymore in Cocoapods or Carthage). Furthermore, I will probably limit styling to the actual look of the controls and don't need many of the positioning features of CSS.

# What's In It? What's Missing?

And then [CiderCSSKit](https://github.com/chsxf/CiderCSSKit) was born. For now, it's mainly a pure lightweight CSS parser.

Here's the list of existing or missing features:

- syntax validation is implemented, but you can create any property with any value you like, without control (`background-image: 10px` for example)
- provides easy access to style properties, in bulk or individually
- named colors are already implemented, but any other keyword will be interpreted as a string
- complex CSS combinators (`>`, `+`, and `~`) are not implemented
- no support for pseudo-classes and pseudo-elements (like `:hover`, `:first-child`, or `::first-line` for example)
- no support for attribute selectors (like `a[target]` or `a[target="_blank"]`)
- built-in functions are limited to `rgb` and `rgba`
- short hexadecimal colors (`#fff` for example) are not supported (colors must use six hexadecimal digits)

# What's Next?

I plan to implement ways to extend the capabilities of the parser, notably with a specific model for custom validation and functions. That will allow the use of CiderCSSKit in other contexts than CiderKit's UI management. I hope that in the future, the package will be agnostic enough to be use by anyone out there. But for now, I have to test it in real situations and improve.

_[Header image](https://commons.wikimedia.org/wiki/File:Cascading_Style_Sheets.jpg) by [Negative Space](https://negativespace.co/) under [Creative Commons CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.en)_
