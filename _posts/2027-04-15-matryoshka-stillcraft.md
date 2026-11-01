---
layout: post
title: "Matryoshka: a present opened layer by layer"
tags:
- android
- stillcraft
- gifts
- encryption
- open source
thumbnail_path: blog/matryoshka-post/hero.jpg
---

I once spent a week choosing a birthday present and two seconds handing it over. A bag, a "happy birthday", paper torn off, done. The present was fine. The moment was over before anyone noticed it had started.

So Matryoshka stretches that moment out. The present itself, or where to find it, sits in the smallest figure of a painted doll. Every figure around it has a note, a photo or a voice inside, and a lock of its own. It's free, with no ads and no internet access at all.

{% include figure.html path=page.thumbnail_path %}

**[Download the APK](https://github.com/bigdevwhale/matryoshka/releases/latest/download/Matryoshka.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/matryoshka/) · [Source on GitHub](https://github.com/bigdevwhale/matryoshka)

## What's inside

### Five kinds of lock

You assemble the doll from the outside in and decide how each figure opens:

- **Time:** on the birthday morning, or an hour after the last one. The phone quietly says when.
- **Riddle:** only the person it's for knows the answer.
- **Place:** it opens only near the café where you met.
- **Someone else's word:** "ask your gran for the code word".
- **Gesture:** cover it with a palm, shake it, turn the phone face down.

When a lock gives, the figure comes apart at the waist, and the next one is already peeking out.

{% include figure.html path="blog/matryoshka-post/shot-1.jpg" caption="The answer is the key." %}

### Riddles that actually hold

A matryoshka travels as a single `.matr` file. Its cover is readable: paintings, names, questions. The insides are encrypted, and each figure holds the key to the next one. For a riddle or a code word, the answer itself becomes part of the key, so the inner figures can't be opened without it, even with the file taken apart. Case, «ё» and punctuation don't matter: "Snowball!" opens what "snowball" locked.

Time, place and gesture are checked by the app on the recipient's phone. A determined person could get round those; they're there for the game, and I say so in the app.

{% include figure.html path="blog/matryoshka-post/shot-2.jpg" caption="It opens only there." %}

### The smallest one

The innermost figure is solid, as in a real matryoshka. It holds the present: tickets, a note saying "look in the wardrobe, top shelf, behind the jumpers", a link. Petals fall when it opens.

{% include figure.html path="blog/matryoshka-post/shot-3.jpg" caption="The present itself." %}

### The workshop

Each figure gets a painting (Khokhloma, Gzhel, Polkhov-Maidan, Semyonov or Vyatka, all drawn in code), a note, a photo or a voice, and a lock. The doll can't be sealed until every riddle has an answer and the smallest figure holds something.

{% include figure.html path="blog/matryoshka-post/shot-4.jpg" caption="Paint, fill, lock." %}

### Together, without a server

A present from a whole group goes round as a file in a set order. The first giver puts in the present and paints the outer figure, then sends the file to the next one. Everyone paints one figure and sees the others only as dark silhouettes. The last one seals it and sends it to the person it's for, so even the givers find out what the others wrote together with the recipient.

{% include figure.html path="blog/matryoshka-post/shot-5.jpg" caption="Your turn: figure 3 of 4." %}

### A word back

Under every figure the recipient can leave a few words or a voice note, and send them all back as a file. Open it, and the replies stand next to the matryoshka you gave.

{% include figure.html path="blog/matryoshka-post/shot-6.jpg" caption="What came back." %}

## Good to know

**Does the recipient need the app?**

Yes. A matryoshka is a file, and only the app opens it figure by figure.

**What does the app send anywhere?**

Nothing by itself. It has no internet permission; you send the file in whatever messenger you like.

**Does it have a dark theme?**

Yes: black lacquer, or light limewood, or follow the system.

## Get it

- **Requires:** Android 11 (API 30) or newer.
- **Permissions:** microphone, for voice notes; location, only for place locks; notifications, for time locks. No internet.
- **License:** MIT. Fonts: Ruslan Display, Lora.

1. Download the APK on your phone.
1. Open it and allow installs from this source when Android asks.
1. Make a matryoshka in the workshop, or tap a `.matr` file someone sent you.

**[Download the APK](https://github.com/bigdevwhale/matryoshka/releases/latest/download/Matryoshka.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/matryoshka/) · [Source on GitHub](https://github.com/bigdevwhale/matryoshka)

---

*Matryoshka is part of [Stillcraft](https://bigdevwhale.github.io/stillcraft/), a shelf of small, free, open-source apps for everyday life: no ads, no accounts, and your data stays on your phone. A new one appears here every few days.*
