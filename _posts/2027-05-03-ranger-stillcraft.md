---
layout: post
title: "Ranger: every run is a patrol on the edge of the kingdom"
tags:
- android
- stillcraft
- running
- open source
thumbnail_path: blog/ranger-post/hero.jpg
---

I run the same few loops through the same park, and after a while every running app shows me the same thing: kilometres, pace, a line that goes up or down. The numbers are useful, but they don't make me want to go out in the rain. I wanted the run itself to matter: the fog over the river, the full moon, the hill I usually avoid.

Ranger is a running journal with a bestiary. You are a ranger of the Borderlands, and every run is a patrol. Who you meet depends on the run: the weather, the sun and moon, the water and woods along the way, and your pulse. It's free, with no ads and no accounts.

{% include figure.html path=page.thumbnail_path %}

**[Download the APK](https://github.com/bigdevwhale/ranger/releases/latest/download/Ranger.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/ranger/) · [Source on GitHub](https://github.com/bigdevwhale/ranger)

## What's inside

### Runs from anywhere

Strava, Garmin, Samsung Health, Mi Fitness, Zepp, Polar and Coros all write to Health Connect, and Ranger reads runs from there with their route and heart rate, along with morning resting pulse, heart-rate variability and daily steps. GPX, TCX and FIT files from any watch work too, gzipped or zipped. A whole Strava archive brings years of runs at once, shoes included. No watch? Record a patrol with the phone's GPS or type a run in by hand. When the same run arrives twice, one copy stays: the one with the track and the pulse.

{% include figure.html path="blog/ranger-post/shot-1.jpg" caption="A patrol: the track, the zones and what it brought." %}

### The bestiary

36 creatures live in the Borderlands, each with its own conditions. The Nixie appears along water, the Moon Lynx in the dark, the Gold-horned Stag at sunrise. The Hill Troll waits on climbs, the Windchaser is only caught with surges, and the Whisperer shows herself to whoever runs quietly in the easy zones. Rare visitors include the Will-o'-the-wisp (night and rain) and the Werewolf (full moon). Legends include the Frost Unicorn at a frozen dawn, the Elder Dragon at thirty kilometres and the Firebird on Midsummer night.

A common creature takes a few patrols: first tracks, then the lair, then the meeting. The weather comes from the run's place and time (Open-Meteo), the sun and moon are worked out on the phone, and the water, parks and woods along the way come from OpenStreetMap.

{% include figure.html path="blog/ranger-post/shot-2.jpg" caption="The Kelpie, a legend: along water, in fog." %}

### The map

The map is drawn on parchment from OpenStreetMap and shows only what your trails have scouted, with seals where creatures were met. Tracks are cut around home and work, so a screenshot doesn't give away where you live.

{% include figure.html path="blog/ranger-post/shot-3.jpg" caption="Only the land you have run." %}

### Valour, ranks and abilities

Valour is training load (TRIMP): time weighted by how high the pulse was, or from pace and climb when there's no pulse. An easy hour and a hard half-hour count about the same. Ten ranks, from Recruit to Legend of the Borderlands, each add something to the outpost: a palisade, a watchtower, a forge, a keep, a beacon on the hill. Nine abilities grow from their own kind of effort, from endurance and night eye to the quiet step and Wayfarer for days at your step goal.

Condition comes from six-week fitness against one-week fatigue. When the load rises too fast or the morning resting pulse is up, the healer suggests a rest.

{% include figure.html path="blog/ranger-post/shot-4.jpg" caption="Valour and nine abilities." %}

### Campaigns, the chronicle and the rest

Campaigns are goals over a few weeks: the Old Road (80 km in a month), the High Pass, the Night Watch, the Quiet Step. The chronicle has the calendar, weekly kilometres, heart-rate zones and the easy share, fitness and fatigue, resting pulse, steps and best times from 1 km to the marathon. Every pair of shoes has its mileage, companions get roles from how you run together, and the bard can note what played in your headphones and tell you what you run faster to.

{% include figure.html path="blog/ranger-post/shot-5.jpg" caption="The board: campaigns for a few weeks." %}

## Good to know

**Does it send my runs anywhere?**

No. Everything stays on the phone. Only map tiles (OpenFreeMap) and weather (Open-Meteo) are requested for the coordinates of your runs. Health Connect is only read, never written.

**I don't have a watch. Will it work?**

Yes. Record a patrol with the phone's GPS or type a run in by hand.

## Get it

- **Requires:** Android 9 (API 28) or newer.
- **Permissions:** Health Connect (read only), location for GPS recording, internet for map tiles and weather. Notification access only if you want the bard to note your music.
- **License:** MIT. Fonts: Cormorant, Alegreya Sans.

1. Download the APK on the phone.
1. Open it and allow installs from this source when Android asks.
1. Connect Health Connect or choose your files, and the first patrols come in.

**[Download the APK](https://github.com/bigdevwhale/ranger/releases/latest/download/Ranger.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/ranger/) · [Source on GitHub](https://github.com/bigdevwhale/ranger)

---

*Ranger is part of [Stillcraft](https://bigdevwhale.github.io/stillcraft/), a shelf of small, free, open-source apps for everyday life: no ads, no accounts, and your data stays on your phone. A new one appears here every few days.*
