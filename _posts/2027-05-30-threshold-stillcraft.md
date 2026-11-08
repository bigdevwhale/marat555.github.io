---
layout: post
title: "Threshold: an alarm that opens the morning"
tags:
- android
- stillcraft
- alarm
- open source
thumbnail_path: blog/threshold-post/hero.jpg
---

Most mornings start the same way: the alarm beeps, you reach for the phone, and five minutes later you're scrolling something you never meant to open. The idea behind Threshold came from someone who wanted the opposite: an alarm that opens the right thing for the day by itself. The news on YouTube on Monday, a playlist on Tuesday, exercise on Wednesday, the radio on Thursday, a film on Friday, a lie-in on Saturday, and silence on Sunday.

So that's what it does. And because a plain list of apps is a dull thing to wake up to, every morning opens through a portal. Threshold is free, with no ads and no account. This is the first version.

{% include figure.html path=page.thumbnail_path %}

**[Download the APK](https://github.com/bigdevwhale/threshold/releases/latest/download/Threshold.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/threshold/) · [Source on GitHub](https://github.com/bigdevwhale/threshold)

## What's inside

### A morning for each day

Each day of the week gets its own time and its own morning: any installed app, a link, or a radio station. A link can be a particular video, playlist, album or film, and it opens right in its own app. Steps chain, too: fifteen minutes of exercise, then the morning show.

The easiest way to fill a day is not to type anything. In YouTube or Yandex Music, tap «Share», pick Threshold, and choose the day.

{% include figure.html path="blog/threshold-post/shot-1.jpg" caption="The week: a world for each day, and what comes through." %}

### Already playing

This took the most fiddling on a real phone. Opening Spotify on a playlist isn't enough: it shows the playlist and waits for you to press play. It turns out Spotify starts playing straight away when given the playlist as `spotify:playlist:<id>:play`, and Yandex Music does the same with `?play=true` on an album link. Threshold rewrites the links you give it into those forms, so the music is already on when the app opens. YouTube videos start with sound by themselves. The one exception is Kinopoisk: it opens the film's page, but has no link that starts the film.

### The portal

When the alarm rings, a portal opens in the arch of your room onto that day's world: the City of Dispatches for the news, the Vinyl Sea for music, the Mountain Stair for exercise, the Antenna Isles for the radio, the Night Cinema for films and the Misty Garden for a slow Saturday. Every world is drawn in code.

The sound is a low hum with bells, synthesized on the phone and rising slowly; on a radio morning the station itself is the alarm. To stop it, you trace a circle with your finger: the portal widens as you go round, and when it holds, you step through. For days when even that is too much, a day can be set to open by itself: the app comes up, and the phone only asks for your fingerprint.

{% include figure.html path="blog/threshold-post/shot-2.jpg" caption="The portal to the Night Cinema, three quarters held open." %}

### Finds

Something falls out of every world: a dispatch feather, a vinyl shard, a ticket stub, a golden reel. Close the day in the evening, at least seven hours before the alarm, and get up at the first ring, and the portal opens wide enough for two. One «five more minutes» narrows it to a slit and leaves one; two leave none. Nothing is ever taken away; the portal just gets narrower.

{% include figure.html path="blog/threshold-post/shot-3.jpg" caption="Through to the cinema: the film opens, two finds fall out." %}

The worlds grow the more you go there, and residents move in: the postmaster, the projectionist, the mountain goat who pretends he isn't racing you. The frame of the arch is rebuilt as finds pile up, from wood and stone to brass, ice and vine.

{% include figure.html path="blog/threshold-post/shot-4.jpg" caption="The map of worlds and the morning journal." %}

### The morning journal

After stepping through, one tap says how waking up went. After a few weeks the journal knows which world wakes you best, and if Mondays with the news are always hard, it suggests giving Monday another world.

### Rules of the week

Public holidays count as days off, a vacation can be set, and «don't wake me tomorrow» is one tap. Once a week, if you allow it, a wild portal opens instead of the usual morning, with something from your «someday» list: the album, the film or the lecture you keep meaning to get to. And twenty minutes after the morning starts, a gentle «time to go», so the morning's video doesn't eat the whole morning.

{% include figure.html path="blog/threshold-post/shot-5.jpg" caption="Finds and the arch's frame." %}

## Good to know

**Will it ring if the phone is locked?**

Yes: the screen wakes and the portal shows over the lock screen. To open an app the phone asks to be unlocked. Phones from Xiaomi and some others close apps at night and wipe their alarms, so Settings has a «Will it ring?» page that shows what the phone still has to allow.

**Why can't the next step of a chain open by itself?**

Android doesn't let an app start another one from the background. The next step comes as a notification, and one tap opens it.

**It's the first version. What if something breaks?**

Please tell me: Settings has buttons to report a bug or suggest an idea through the StillCraft bot.

## Get it

- **Requires:** Android 9 (API 28) or newer.
- **Permissions:** alarms and notifications, showing over the lock screen, starting after a reboot; the internet only for radio stations you choose.
- **License:** MIT. Fonts: Cormorant Garamond and Inter (OFL).

1. Download the APK on the phone.
1. Open it and allow installs from this source when Android asks.
1. Give each day its morning, then check «Will it ring?» in Settings.

**[Download the APK](https://github.com/bigdevwhale/threshold/releases/latest/download/Threshold.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/threshold/) · [Source on GitHub](https://github.com/bigdevwhale/threshold)

---

*Threshold is part of [Stillcraft](https://bigdevwhale.github.io/stillcraft/), a shelf of small, free, open-source apps for everyday life: no ads, no accounts, and your data stays on your phone. A new one appears here every few days.*
