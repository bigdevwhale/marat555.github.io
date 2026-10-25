---
layout: post
title: "Pilgrim: Ten minutes at a place of power. A stamp for each."
tags:
- android
- stillcraft
- mindfulness
- digital-detox
- openstreetmap
- open source
thumbnail_path: blog/pilgrim-post/hero.jpg
---

There's an old church two streets from my flat. For years I walked past it with the phone in my hand. When I finally went in and sat down, I lasted about ninety seconds before I checked something. The place was fine. I was the problem. I didn't need another meditation timer. I needed a reason to walk there, and a reason not to touch the phone once I did.

Pilgrims had that reason: a little book stamped at every holy place they reached. Pilgrim is that book. Come to a place of power, sit ten minutes without the phone, and the book gets a stamp nobody else has. Free, no ads, no account.

{% include figure.html path=page.thumbnail_path %}

**[Download the APK](https://github.com/bigdevwhale/pilgrim/releases/latest/download/Pilgrim.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/pilgrim/) · [Source on GitHub](https://github.com/bigdevwhale/pilgrim)

## What's inside

### Places of power, from the common map

OpenStreetMap already knows the places: houses of worship of every faith, wayside chapels, springs, protected trees, named stones, peaks and viewpoints. When you're within a place's radius, the book says so. A big mosque courtyard counts as a whole. Under trees and in ravines, where GPS wanders, a place gets more room.

### Ten minutes, honestly

You tap "I'm leaving the phone" and turn the screen off. The time runs only while the screen is dark. A foreground service watches three things:
- **the screen:** a glance at a notification is forgiven, but ten seconds of a lit screen means you picked it up;
- **the place:** fixes far away for a minute and a half mean you left;
- **the motion sensor:** a minute of walking with the phone means you're not sitting.

A faked location never counts. After ten minutes, two soft pulses, and the stamp is yours whatever happens next. You may stay as long as you like.

{% include figure.html path="blog/pilgrim-post/shot-1.jpg" caption="The sitting: the screen dark, the time running." %}

### No two stamps alike

The stamps are inked in code. The frame belongs to the place: round, oval, octagonal or scalloped. The silhouette shows its kind: a dome, a minaret, a pagoda, a drop of spring water, a tree. The number is your visit. The ink comes from where the sun was: gold at dawn, ochre by day, cinnabar at sunset, indigo at night. The weather shows on the paper. Rain bleeds the ink, snow leaves frost, wind smears a second strike and fog fades it. Twenty minutes add a ring, forty a ring of beads.

{% include figure.html path="blog/pilgrim-post/shot-2.jpg" caption="A page of the book: four stamps, two words under each." %}

### Two words, and a note in the crevice

After the sitting, the book asks two questions: what did you leave here, and what did you take? You answer with one word each, a grudge and silence, or hurry and air. You can also leave a note in the crevice. It opens only at that place, on another day, after ten minutes.

### A place of your own

Not every place of power is on a map: a bench by the water, a hill, the yard you grew up in. You can mark one from a good GPS fix, but not within 300 m of home, and no more than two a week. It goes into the book in pencil and is inked on your third day there. If it's a spring or a chapel, the book offers to leave an anonymous note for the OpenStreetMap mappers.

### This week's rumour

The book never shows you the whole map. Once a week it tells you about one place nearby, the way people do: "They say there's a spring ten minutes' walk north-west of home. The water is cold even in July." On the map it's a vague circle with a question mark, and you have to find the spring yourself.

{% include figure.html path="blog/pilgrim-post/shot-5.jpg" caption="At the Marjani Mosque, with this week's rumour below." %}

### Walks, the year's circle and the scroll

Three places in a day make a walk: a page of its own, with the stamps on one thread. Sit at a place in winter, spring, summer and autumn, and its circle closes; you give it a name of your own. Home's places fill the first notebook, and each faraway town starts another one under its own leather cover. At the end of the year, the scroll rolls it all up: the time in silence, the walks, a little map of each town and every pair of words.

{% include figure.html path="blog/pilgrim-post/shot-3.jpg" caption="A walk in St Petersburg: a synagogue, a cathedral, a mosque and a Buddhist temple in one day." %}

{% include figure.html path="blog/pilgrim-post/shot-4.jpg" caption="A bench by the lake, sat at in all four seasons and named Still Water." %}

## Good to know

**Is it religious?**

No. It's about going somewhere quiet and staying there without the phone. Before the first sitting at each kind of place, the book gives a few lines on how to come in: where to take off your shoes, when to sit in the courtyard instead. The tone is the same for every faith, and there are no symbols of faith anywhere, only buildings and nature.

**What leaves the phone?**

Only a rounded point: it asks OpenStreetMap for places nearby and the weather services for the weather on the stamp. Home is worked out on the phone, from where it is at night. Your words, notes and home never leave it.

## Get it

- **Requires:** Android 9.0 (API 28) or newer. No Google services needed.
- **Permissions:** location, a notification for the sitting, vibration, and the internet for the map of places and the weather.
- **License:** MIT

1. Download the APK on your phone.
1. Open it and allow installs from this source when Android asks.
1. Walk to the nearest church, mosque, spring or old tree, tap "I'm leaving the phone" and turn the screen off.

**[Download the APK](https://github.com/bigdevwhale/pilgrim/releases/latest/download/Pilgrim.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/pilgrim/) · [Source on GitHub](https://github.com/bigdevwhale/pilgrim)

---

*Pilgrim is part of [Stillcraft](https://bigdevwhale.github.io/stillcraft/), a shelf of small, free, open-source apps for everyday life: no ads, no accounts, and your data stays on your phone. A new one appears here every few days.*
