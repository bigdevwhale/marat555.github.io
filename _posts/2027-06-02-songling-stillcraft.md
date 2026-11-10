---
layout: post
title: "Songling: a pet that listens to music with you"
tags:
- android
- stillcraft
- music
- open source
thumbnail_path: blog/songling-post/hero.jpg
---

Familiar raises a pet on the pages you read, Reelkin on the films you watch. Songling does it with music, and it asks even less of you: there's nothing to log. Play music in any player and the pet hears it, dances along and grows with every minute.

Around the pet there's a shelf of everything you've played, stats for any span, goals and a little pixel den to furnish. Songling is free, with no ads and no account. This is the first version.

{% include figure.html path=page.thumbnail_path %}

**[Download the APK](https://github.com/bigdevwhale/songling/releases/latest/download/Songling.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/songling/) · [Source on GitHub](https://github.com/bigdevwhale/songling)

## What's inside

### It hears any player

Android only tells another app what a player is playing if that app has notification access, so Songling asks for it once. It reads no notifications: it only looks at the media session of whatever plays – Spotify, YouTube Music, Apple Music, Poweramp – and takes the song, the artist and the album. A song counts after 30 seconds, and anything longer than forty minutes is taken for a podcast and left out.

Genres, years, track lists and covers come from iTunes, artists' countries from MusicBrainz. A record, the radio or a concert can be logged by hand: a whole album, or one song from it or on its own.

### 18 pets

There are three eggs to start with. The others turn up by the way you listen: ten hours of rock bring the Bassilisk, five nights of music after ten bring the Cricket, artists from five countries bring the Wandering Hedgehog, and a live concert brings the Firefly.

{% include figure.html path="blog/songling-post/shot-2.jpg" caption="The bestiary: every pet you've raised, and the eggs still to find." %}

An egg hatches after three hours of music, and the pet grows from a baby to an ancient one. Each loves something: jazz, whole albums in order, headphones, music before ten in the morning. Its favourite music feeds it one and a half times as much and pays double plectrums, the den's money. Each also has a nature, and some natures do something: a greedy one grows faster, a curious one earns a plectrum for every new artist. A grown pet takes one of six forms for good, by the music it was raised on.

{% include figure.html path="blog/songling-post/shot-3.jpg" caption="A pet's card: what it loves, what it can do, and what it has eaten." %}

### The den

The pet lives in a small pixel room. While music plays it dances: it nods to rock, bounces to dance music and sways slowly to ambient. Days without music wind it down until it stops like a clockwork toy; a couple of songs and it moves again.

The window shows the real weather and time of day in your town, from Open-Meteo. Plectrums buy a turntable, a record shelf that fills with the colours of the albums you've heard whole, fairy lights, a disco ball and a dozen more things, each tried in the room before you pay. Two frames on the wall take posters: your favourite album and a band.

{% include figure.html path="blog/songling-post/shot-5.jpg" caption="Trying things in the den before buying them." %}

### The shelf

Everything you've heard: albums, singles, songs and artists, with covers, years, genres, stars and notes. A «to hear» list keeps what you never get round to, and hearing it pays plectrums. Genres can be your own, like «for work» or «grandma's records».

Albums look like records in sleeves or like tapes with hand-written J-cards – you choose which on first start. In an album's card the record slides out of its sleeve, and the songs are split into side A and side B.

{% include figure.html path="blog/songling-post/shot-1.jpg" caption="An album: the sleeve, the numbers, and the songs by side." %}

### Saga and goals

The saga shows a week, a month, a year or all time: the top artists, albums, songs and genres, the hours and weekdays you listen most, countries and decades, and a calendar with every day of the year. Filters narrow it to a genre, a player or headphones only.

{% include figure.html path="blog/songling-post/shot-4.jpg" caption="A month in numbers." %}

Goals can be about anything: four new albums a month, ten hours of one band this year, music from three new countries. And every week brings three small tasks, like an album heard whole or an artist you haven't played for half a year.

## Good to know

**Why does it need notification access?**

It's the only way Android lets one app see what another is playing. Songling takes the title, the artist and the album, and nothing else.

**Can I bring my history over?**

Yes: Spotify's extended streaming history, Last.fm as CSV and ListenBrainz exports all go into the stats and the shelf. The pet doesn't eat old music, only what it hears with you.

**It's the first version. What if something breaks?**

Please tell me: Settings has buttons to report a bug or suggest an idea through the StillCraft bot.

## Get it

- **Requires:** Android 9 (API 28) or newer.
- **Permissions:** notification access, only to see what plays; the internet for covers, genres, countries and the weather in the window.
- **License:** MIT. Fonts: Comfortaa, Nunito, Caveat and IBM Plex Mono (OFL).

1. Download the APK on the phone.
1. Open it and allow installs from this source when Android asks.
1. Pick an egg, allow notification access, and play some music.

**[Download the APK](https://github.com/bigdevwhale/songling/releases/latest/download/Songling.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/songling/) · [Source on GitHub](https://github.com/bigdevwhale/songling)

---

*Songling is part of [Stillcraft](https://bigdevwhale.github.io/stillcraft/), a shelf of small, free, open-source apps for everyday life: no ads, no accounts, and your data stays on your phone. A new one appears here every few days.*
