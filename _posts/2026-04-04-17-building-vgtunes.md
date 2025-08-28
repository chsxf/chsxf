---
layout: post
title: "Building VGTunes"
image: /assets/posts/17/make-it-good-later.jpg
date: 2026-04-04
tags:
  - VGTunes
description: >
  At the end of 2024, I was stuck in the mud with Swift's String Concurrency. After some time, I finally realized I needed a break from all that for a while. That "while" lasted almost 6 months and resulted in VGTunes. In this post, I will talk about the creation of my own video game music database website.
---

It's been a long time since I posted on this devlog. It seems that's happening when you are fully immersed in the last stages of production of a video game.

At the end of 2024, I was stuck in the mud with [CiderKit](https://github.com/chsxf/CiderKit). I wasn't able to wrap my head around [Strict Concurrency](https://developer.apple.com/documentation/Swift/AdoptingSwift6) becoming mandatory with Swift 6, and I wanted to make my project compliant with that. I ended up making some nice refactoring within the map management classes, but forcing myself into it was a mistake. I finally realized I needed a break from all that, and decided to stick with Swift 5 for the time being. And to work on something else for a while. Though, that moment lasted almost 6 months and gave me [VGTunes].

The first version was released more than a year ago, on January 15th, 2025.

![Make it exist first, Make it good later](/assets/posts/17/make-it-good-later.jpg)

# VGWhat?

I am passionate about pretty much everything in video games. From tech to art to design, I love it all. But, besides the actual games themselves, soundtracks are what I consume the most. And, unfortunately, video game soundtracks, however famous they may be today, did not always receive the recognition they deserved. Case in point, most streaming platforms lack a dedicated "Video Games Music" category, or at best have a curated subsection that contains only a few entries. I always said to myself that, given the opportunity, I would work towards a better platform for video game music.

Another aspect of today's digital music landscape, one that is not tied to video game music specifically, is that many people are using either [Spotify](https://open.spotify.com) or [Apple Music](https://music.apple.com). That's not my case, and I became somewhat tired of being a [Deezer](https://deezer.com)[^deezer] user confronted with another significant part of the world sharing links from other services, making them essentially useless for me. What if there were a website listing video game soundtracks and their location on the various streaming platforms? A database of streamable video game soundtracks if you will.

That became the starting point for [VGTunes], a website where you can find video game _(hence the VG)_ music _(Tunes)_, and where it is streamable from. Plus other infos, like the Steam page for the game.

# Finding the Intersection

Other people are already building similar things.

First, you have individuals building more editorial oriented websites like [NOWPLAYING](https://nowplaying.cool/). But that's not my goal. I'm lacking the time for such authored content.

On one hand, you have [VGMdb](https://vgmdb.net): a proper database of everything video game music, including physical editions. The website is fairly complete, with detailed information on every entry. Though in many cases, links to streaming services are missing. This is the reference website and my main target. I hope at some point in the future [VGTunes] will be as detailed and complete.

On the other hand, you have social websites like [Songlink/Odesli](https://odesli.co/) and [Listen.to](https://li.sten.to/). However, these websites are not databases. They only allow people and artists to create custom webpages with links to the various avenues where a specific album or song is available, acting as a single URL to use on their website.

[VGTunes] is basically a mix of these. It's a database, expected to be as complete as possible, but with the design philosophy of the social websites.

![Moss II VGTunes Link Example](/assets//posts/17/link-example.jpg)

# Starting Small

As I write this post, [VGTunes] references more than 2,300 albums and 1,200 artists. More than a year after the start of the project, it's already a substantial database. Of course, it is nowhere near completion as I had to start from nothing.

How do you fill up a database of streamable video game soundtracks? For some reason, streaming platforms have no comprehensive way to search by genre, or even a dedicated genre for video game soundtracks. Even worse, some albums have no genre attached. That couldn't be the starting point, nor a valid source for automation.

I turned to VGMdb and search for an API. In late 2024, nothing concrete was available. Right now, in April 2026, an API seems to be on its way, but is still in private beta and unvailable to me at the moment.

So, I had to build my own tools to populate the database.

# Tooling Up

I took a very naive approach: search the streaming platforms for generic keywords ("original game soundtrack" for example) and add each result manually. The very first version of this search/add tool was very rudimentary. The tool was searching on Deezer and I had to manually get the IDs for other platforms by hand (Apple Music and Spotify only at the time).

At the time of writing, the website supports Apple Music, [Bandcamp](https://bandcamp.com/), Deezer, Spotify, [Tidal](https://tidal.com/), as well as [Steam](https://store.steampowered.com/) and links to Steam soundtracks when available. It was unimaginable to be able to scale with such limited helpers.

So, I refined the process over time to automate as many steps as possible. The starting point is still pretty much the same: search for an album on one of the platforms and select it for inclusion in the database. But after that, the current backend of [VGTunes] actually scans other platforms for similar results and automatically link albums if an exact match is found. It saves me a lot of time and helped grow the database much faster. It is still far from perfect, but it works.

![VGTunes Dashboard](/assets/posts/17/vgtunes-dashboard.jpg)
_VGTunes Backend Dashboard_

I also created a website generator (as the front-end of [VGTunes] is basically a static website) that takes information from the database and creates the various web pages for every referenced album and artist, including alphabetical catalog pages.

# What's Next?

There is probably a visual redesign down the line for [VGTunes]. And more platforms added to it, like [Qobuz](https://www.qobuz.com), [YouTube Music](https://music.youtube.com/), and others.

On the feature side, I would like to create browser extensions that would allow sharing [VGTunes] links when listening to a referenced album on the supported streaming platforms.

Only time will tell what comes first.

[VGTunes]: https://vgtunes.chsxf.dev

[^deezer]: About a year after Spotify was founded in 2006, a company called Deezer was born in France. And it became progressively very popular in the country, in part due to the fact that the first french internet service provider at the time (Orange) got invested in it. Being french, I became a subscriber a long time ago (at the same time as my internet service) and, after all these years, this is still the streaming platform from which I get my music. _And, of course, there are also so [many](https://time.com/6144634/artists-boycott-spotify-joe-rogan/) [legitimate](https://www.latimes.com/entertainment-arts/music/story/2025-07-31/spotifys-ceo-owns-an-ai-weapons-company-some-musicians-say-its-time-to-leave) [reasons](https://www.forbes.com/sites/bernardmarr/2025/03/04/spotifys-bold-ai-gamble-could-disrupt-the-entire-music-industry/) not to subscribe to Spotify._
