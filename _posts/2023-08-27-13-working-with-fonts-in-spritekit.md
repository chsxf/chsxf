---
layout: post
title: "Working With Fonts in SpriteKit"
image: /assets/posts/13/fonts-spritekit.png
date: 2023-08-27 12:00:00 +0100
excerpt: >
  Documentation for SpriteKit is sparse. Many parts of the framework are pretty obscure, with pretty much no information available out there. That’s the case for fonts. In this post, I explain how to work programmatically with fonts in SpriteKit.
---

![Fonts with SpriteKit](/assets/posts/13/fonts-spritekit.png)

Documentation for [SpriteKit](https://developer.apple.com/spritekit/) is sparse. Many parts of the framework are pretty obscure. So that's no surprise if many solutions can be found outside of Apple on personal blogs, or [StackOverflow](https://stackoverflow.com/questions/tagged/sprite-kit) and the likes. However, there are some very specific areas that are even more obscure, with pretty much no information available out there. That's the case for fonts.

# The Simple Way

If you are using the [SpriteKit Scene Editor](/2023/02/11/9-when-xcode-meets-spritekit.html) embedded in Xcode, this post won't interest you much. Fonts are handled pretty easily like on any other [WYSIWYG](https://en.wikipedia.org/wiki/WYSIWYG) editor. You choose your font, with its specific style and weight, and it appears inside your [SKLabelNode](https://developer.apple.com/documentation/spritekit/sklabelnode) as expected.

The only thing you may need to do if you use a custom font that is not pre-installed with macOS is to bundle it with your app and [declare](https://developer.apple.com/documentation/bundleresources/information_property_list/atsapplicationfontspath) [its usage](https://developer.apple.com/documentation/uikit/text_display_and_fonts/adding_a_custom_font_to_your_app) to the system. But beyond that, you're good to go.

# A Family Matter

Unfortunately, that's not the same story if you want to handle fonts programmatically. To identify the font to use on a specific SKLabelNode, SpriteKit provides you with a single property only: [`fontName`](https://developer.apple.com/documentation/spritekit/sklabelnode/1520129-fontname).

This cryptic field hides in fact a lot of information. But first we have to take a step back and understand how fonts actually work.

Fonts are divided into families. Each family has a different name. Some popular font families are for example **Arial** or **Times New Roman**. Then, in a given family, you have variants for each style (italic, oblique, condensed, etc) and weight (light, bold, regular, etc). Not all font families contain all style and weight variants though. And to make matters worse there is no standardization when it comes to variant names.

For example, the italic version of Times New Roman is called _TimesNewRomanPS-ItalicMT_. Whereas it is _Arial-ItalicMT_ for Arial. Or _Baskerville-Italic_ for the Baskerville font family. There are similarities, but this is not 100% consistent.

# Ask the Oracle

If it wasn't confusing enough, SKLabelNode's `fontName` property accepts two possible values: a font family (ie `Times New Roman`) or a font variant name (ie `TimesNewRomanPS-ItalicMT`). If you set the property with a font family, the default regular font variant will be used. But how to guess the font variant name then if you want to use italic or bold, or even both?

If you are using only a few fonts, you can get that information from the **Font Book** app and use it directly into your code. For that, select a font variant and display its information. In the right panel, you should be given access to an **Identifier** section (see screenshot below).

![Font Book app showing a font variant name](/assets/posts/13/font-book-identifier.png)
_Font Book app showing a font variant name_

However, in my case, [CiderKit](https://github.com/chsxf/CiderKit) is an engine. It should be able to handle any font. If you're facing a similar issue, that means you have to query the system's font management thanks to either [`NSFontManager`](https://developer.apple.com/documentation/appkit/nsfontmanager/) on macOS or [`UIFontDescriptor`](https://developer.apple.com/documentation/uikit/uifontdescriptor/) anywhere else.

Below is a Swift script you can use to get the right font variant name for SpriteKit. It is currently limited to regular, italic, bold, or bold italic variants, but should be pretty easy to extend if you need to.

```swift
#if os(macOS)
import AppKit
#else
import UIKit
#endif

final class FontHelpers {

    class func fontName(with fontFamily: String, italic: Bool, bold: Bool) -> String? {
        getFontName(fontFamily, italic, bold)
    }

    #if os(macOS)

    fileprivate class func getFontName(_ fontFamily: String, _ italic: Bool, _ bold: Bool) -> String? {
        var traits: NSFontTraitMask = []
        if italic {
            traits.insert(.italicFontMask)
        }
        if bold {
            traits.insert(.boldFontMask)
        }
        let font = NSFontManager.shared.font(withFamily: fontFamily, traits: traits, weight: 5, size: 12)
        return font?.fontName
    }

    #else

    fileprivate class func getFontName(_ fontFamily: String, _ italic: Bool, _ bold: Bool) -> String? {
        var descriptor = UIFontDescriptor().withFamily(fontFamily)
        if bold || italic {
            var traits: UIFontDescriptor.SymbolicTraits = []
            if italic {
                traits.insert(.traitItalic)
            }
            if bold {
                traits.insert(.traitBold)
            }
            if let newDescriptor = descriptor.withSymbolicTraits(traits) {
                descriptor = newDescriptor
            }
        }
        let font = UIFont(descriptor: descriptor, size: 12)
        return font.fontName
    }

    #endif

}
```
