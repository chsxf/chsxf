---
layout: post
title: "It's Not a Bug. It's an URL Feature"
date: 2022-07-18 12:00:00 +0100
image: /assets/posts/4/foundation-framework.png
excerpt: >
    As always, the bug was between the chair and the keyboard. In this post, I discuss the weird bug I encountered in my code with the counter-intuitive URL struct initializers of the Foundation framework.
---

Let's take a look at this snippet of Swift code:

```swift
let homeDirectoryURL = FileManager.default.homeDirectoryForCurrentUser
let folderURL = URL(fileURLWithPath: "MyFolder", relativeTo: homeDirectoryURL)
let fileURL = URL(fileURLWithPath: "MyFile.txt", relativeTo: folderURL)
let absoluteFileURL = fileURL.absoluteURL
```

At a glance, you would intuitively think `absoluteFileURL` is equal to the like of `file://Users/chsxf/MyFolder/MyFile.txt`. And so did I.

Obviously, I was wrong. The result is in fact `file://Users/chsxf/MyFile.txt`.

That created a weird bug in my code while working on [The Untitled Project](/2022/01/15/2-the-untitled-project.html). The FileManager's `createDirectory(at:withIntermediateDirectories:attributes:)` method would only create the last folder relative to my base URL, and not all parent sub-folders, even though the final URL was iteratively built as relative to every one of them.

After digging into this issue for several hours, I started to think there was a bug in the Foundation framework. But, as I was firing the Feedback Assistant to report the issue to Apple, I discovered the `URL(fileURLWithPath:isDirectory:relativeTo:)` variant of the `URL` initializer.

In my case, the right solution was:

```swift
let folderURL = URL(fileURLWithPath: "MyFolder", isDirectory: true, relativeTo: homeDirectoryURL)
```

As always, the bug was between the chair and the keyboard. The problem was that the last path component of my relative URL was considered a file name. And creating another URL relative to this same URL was only replacing that last component.

However, as wrong as I was, URLs and those initialisers can be very counter-intuitive. So I hope this small post will help some of you avoiding this kind of issues.