---
layout: post
title: "Cabinet of Curiosities: a museum for an odd collection"
tags:
- android
- stillcraft
- collecting
- open source
thumbnail_path: blog/cabinet-post/hero.jpg
---

People collect strange things: 0.33 cans, crocodiles of every material, fridge magnets, Soviet records. The usual way to keep track is a spreadsheet, and a spreadsheet is the dullest possible home for a collection. Worse, it doesn't help at the moment that matters most: at a flea market, holding a can, trying to remember whether you already have this one.

Cabinet of Curiosities gives a collection its own museum, with halls, cabinets, a card for every thing, and a photo check: do I already have it? It's free, with no ads, no account and no internet permission. This is the first version.

{% include figure.html path=page.thumbnail_path %}

**[Download the APK](https://github.com/bigdevwhale/cabinet/releases/latest/download/Cabinet.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/cabinet/) · [Source on GitHub](https://github.com/bigdevwhale/cabinet)

## What's inside

### Do I have it?

Photograph a thing at a flea market or a fair, and the app compares it with every hall, wish list included. The answer is one of three: you already have it, most likely; there are similar ones; or you don't have this, so it's worth taking. It also tells you when the thing is on your wish list. The comparison runs on the phone, and a long press on the app icon opens the check straight away, before the seller loses patience.

{% include figure.html path="blog/cabinet-post/shot-1.jpg" caption="A 1924 rouble: already in the collection." %}

### Halls and cabinets

Each collection gets a hall, and each hall has cabinets. Records, books, coins, cans, figurines, magnets, stamps and model cars come ready, with the card fields that kind of collection needs: artist and label for a record, denomination and metal for a coin, publisher and binding for a book. A hall of your own takes whatever fields you invent.

{% include figure.html path="blog/cabinet-post/shot-2.jpg" caption="The records hall, with cabinets along the top." %}

### A card for every thing

Photos, an inventory number, where it came from, when and for how much, where it's kept, its condition and its story. Things come in through the phone's own camera or a dozen at a time from the gallery, and the app cuts each one out of the background, so the shelves look like a museum, not a camera roll.

A thing with an empty card stays faded. Write where it came from or what you remember about it, and it gets its colour back. Series show which places are still empty, and duplicates go to the swap fund.

{% include figure.html path="blog/cabinet-post/shot-3.jpg" caption="A card, and what the visitors said about it." %}

{% include figure.html path="blog/cabinet-post/shot-4.jpg" caption="Editing a card. The scissors bring the background back." %}

### Why come back

The museum grows with the collection, from a cabinet in the hall to a museum with a crest, and described things raise your rank from keeper of a cabinet to museum director. Every evening it asks one question about one thing, and the answer goes into the card. Visitors come every day and linger at the best described things, leaving a line in the visitors' book. Each week there is an exhibition to put on, from everything red to the old-timers, with a poster to share. Once a season there's an inventory with a wax seal, and the arrivals book keeps every thing in the order it came.

{% include figure.html path="blog/cabinet-post/shot-5.jpg" caption="The arrivals book." %}

The demo museum is built from real records, coins and books photographed by people on Wikimedia Commons; every photo is credited in the repository.

## Good to know

**How sure is «you have it»?**

The lens compares the shape and picture of a thing, not its text. Two coins of the same year and design look the same to it, so check the card it shows: number, cabinet, where it's kept.

**Can I move my collection to a new phone?**

Yes. Settings makes a backup file with all the photos. There's also a PDF catalogue and a CSV table.

**It's the first version. What if something breaks?**

Please tell me: Settings has buttons to report a bug or suggest an idea through the StillCraft bot.

## Get it

- **Requires:** Android 9 (API 28) or newer.
- **Permissions:** none worth mentioning. Photos go through the phone's own camera and the system picker, and there's no internet permission at all. The background cut-out model comes with Google Play services on first use.
- **License:** MIT. Font: Manrope (OFL).

1. Download the APK on the phone.
1. Open it and allow installs from this source when Android asks.
1. Name the museum, open the first hall and bring in a few things.

**[Download the APK](https://github.com/bigdevwhale/cabinet/releases/latest/download/Cabinet.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/cabinet/) · [Source on GitHub](https://github.com/bigdevwhale/cabinet)

---

*Cabinet of Curiosities is part of [Stillcraft](https://bigdevwhale.github.io/stillcraft/), a shelf of small, free, open-source apps for everyday life: no ads, no accounts, and your data stays on your phone. A new one appears here every few days.*
