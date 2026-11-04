---
layout: post
title: "Fieldscan (Beta): an anomaly scanner for your own city"
tags:
- android
- stillcraft
- walking
- open source
thumbnail_path: blog/fieldscan-post/hero.jpg
---

I walk the same streets every evening and stopped noticing them years ago: the footbridge over the canal, the fountain nobody drinks from, the bus stop where nobody ever seems to wait. Location games exist, but they put a monster on a map and ask you to tap it. I wanted the opposite: a reason to stand still in a place for a minute and look at it.

Fieldscan turns the phone into a field device for observing anomalies. Creatures live in the city, each in its own kind of place, and each comes out only to someone who works out its riddle and does its rite. It's free, with no ads and no accounts. This is a beta.

{% include figure.html path=page.thumbnail_path %}

**[Download the APK](https://github.com/bigdevwhale/fieldscan/releases/latest/download/Fieldscan.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/fieldscan/) · [Source on GitHub](https://github.com/bigdevwhale/fieldscan)

## What's inside

### Only a trace

The round scanner never shows a creature. It hears a trace: the kind of place, the distance, the direction, a scrap of signal like "…splash… under… step…" and its strength. The places are real and come from OpenStreetMap around you: bridges over running water, springs and fountains, long stairs, bus stops, pharmacies, street lamps, crossroads of five roads, churches, piers. A place always holds the same creature, and about four in ten fitting places are empty, so not every lamp is haunted.

{% include figure.html path="blog/fieldscan-post/shot-1.jpg" caption="The air: traces around you." %}

### The riddle and the rite

Each trace opens a riddle: "Heard under bridges. Comes out to someone standing right in the middle, facing upstream. Doesn't like fuss." The riddle tells the rite, and every step is checked by the phone. The middle of the span by GPS, upstream by the compass (the app knows which way the water runs under each bridge), keeping still by the accelerometer, a sung A by the pitch of your voice (nothing is recorded), a letter blinked in Morse with the torch, three rounds of a crossroads anticlockwise, a staircase climbed without stopping. Slip up and the step starts over; steps already done are kept for two hours.

{% include figure.html path="blog/fieldscan-post/shot-2.jpg" caption="The rite: conditions and steps." %}

### The trail camera

When the rite is done, a trail camera fires: a grainy night infrared frame with the camera number, the date and the time. There are no drawings in the app. Each creature is grown in code from a skeleton of bones, its outline torn by noise, given fur and texture, and half hidden by its place: grass in front of its feet, the stone rim of a well, the glass of a bus shelter. The hour, fog, rain, snow and frost all show in the frame, so two people's Bridgelings never look the same.

{% include figure.html path="blog/fieldscan-post/shot-3.jpg" caption="The bestiary: frames and riddles." %}

### Keepers, wanderers and rumours

Keepers live in the district's most-of-all places, which the phone finds by itself: the oldest tree, the longest stairs, the longest bridge, the busiest crossroads. Wanderers come only with the weather: the Fogling walks the riverbank in fog, the Rainling hides under bus shelters in the rain, the Gustling stops by you in a strong wind, First-Snow follows your tracks in a snowfall, the Moon Dog runs the open places under a full moon. Every catch tells a rumour about someone else, so the bestiary grows as a web: the Bridgeling is afraid of the Gudgeon, the Keeper of the Long Stairs waits for someone carrying a Vanecrow.

{% include figure.html path="blog/fieldscan-post/shot-4.jpg" caption="A riddle: someone is there." %}

### Memory and the archive

The device holds 16 KB. Frames in memory wear a little every day, the grain grows and bands creep in, until you take them home and hand them in to the archive, where they stay as they were. New catches bring parts, and parts buy a longer antenna and more memory. A page of the device's manual explains each screen the first time you open it, and a dark theme turns the screen to night mode.

{% include figure.html path="blog/fieldscan-post/shot-5.jpg" caption="Rumours: who tells about whom." %}

## Good to know

**Do I have to walk at night?**

No. Many creatures come by day, at dawn or at dusk, and night creatures can be switched off.

**What if I don't want to sing in the street?**

Turn on the quiet way: instead of a note, the rite asks you to keep still for twenty seconds.

**Why a beta?**

There are 50 creatures so far; the rest of the planned two hundred will come in updates, and the rites need checking on real walks.

## Get it

- **Requires:** Android 9 (API 28) or newer.
- **Permissions:** location, the microphone only for the sung note, internet for the map (OpenStreetMap) and the weather (MET Norway). Coordinates stay on the phone.
- **License:** MIT. Font: m6x11 by Daniel Linssen.

1. Download the APK on the phone.
1. Open it and allow installs from this source when Android asks.
1. Allow location, mark home as the archive, and catch the first creature right at your door.

**[Download the APK](https://github.com/bigdevwhale/fieldscan/releases/latest/download/Fieldscan.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/fieldscan/) · [Source on GitHub](https://github.com/bigdevwhale/fieldscan)

---

*Fieldscan is part of [Stillcraft](https://bigdevwhale.github.io/stillcraft/), a shelf of small, free, open-source apps for everyday life: no ads, no accounts, and your data stays on your phone. A new one appears here every few days.*
