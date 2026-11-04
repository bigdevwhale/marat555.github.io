---
layout: post
title: "Your Move: chess by post"
tags:
- android
- stillcraft
- chess
- open source
thumbnail_path: blog/yourmove-post/hero.jpg
---

Online chess is always in a hurry: a clock ticking, an opponent you'll never hear from again, a rating going up and down. I wanted the opposite: one move a day, thought over with a cup of tea, sent to someone you slowly get to know. People used to play like that by post, for months.

Your Move is chess by post. People from far corners of the world write to you, and every letter carries a move and a little of their life. You answer with a move and a few words, seal the letter, and the reply comes with the post. It's free, with no ads, no accounts and no internet. This is the first version.

{% include figure.html path=page.thumbnail_path %}

**[Download the APK](https://github.com/bigdevwhale/yourmove/releases/latest/download/YourMove.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/yourmove/) · [Source on GitHub](https://github.com/bigdevwhale/yourmove)

## What's inside

### Seven correspondents

Three write to you at once. Timosha is nine, goes to a chess club in Petrozavodsk and has a hamster called Gambit. Vera Pavlovna taught maths in Vologda for forty years and has a cat called Queen. Sigurd keeps the last lighthouse on Nólsoy in the Faroes, and his letters come with the mail boat twice a week.

As you finish games, your address is passed along, and others write too: Mateo, an architecture student who plays in a café in Buenos Aires; Hanna, a night nurse in Tromsø who makes one move on her three o'clock break; Brother Anselm, a beekeeping monk in the Swiss mountains; and Kenji Okada, a watchmaker in Kyoto.

Each one's story is told across eight letters. The lighthouse is going automatic in March. The café may close. The sun comes back to Tromsø after the polar night. Sometimes they ask you something, and the next letter answers your answer.

{% include figure.html path="blog/yourmove-post/shot-1.jpg" caption="A letter from Mateo, and a copy of yours." %}

### A character on the board

They don't all play the same. Sigurd loves knights and won't trade queens. Mateo sacrifices pieces, sometimes even on purpose. Hanna trades down into endgames because "in an endgame you can't hide behind pieces". Brother Anselm locks the position with pawns. Kenji guards his two bishops "like the hour and minute hands". Timosha brings his queen out far too early.

The rules and the engine are written from scratch in Kotlin. Each correspondent's tastes go straight into how they evaluate a position, and their strength follows yours: win, and they play a step stronger next game; lose, and they ease off. Their slips are human, but none of them will miss a mate in one, and they resign a hopeless position themselves.

{% include figure.html path="blog/yourmove-post/shot-2.jpg" caption="Your move: a piece and where it can go." %}

### The post takes time

Ordinary post comes at 8, 13 and 19, like real correspondence. If you can't wait, there's express (about an hour) and pigeon post (a few seconds). The phone tells you when a letter has come.

Until you seal the letter you can change your mind. The kitchen board lets you move for both sides and try lines; it's a scratchpad, and nothing from it goes into the game.

### Stamps, parcels and pieces that remember

Every envelope has a stamp, four from each correspondent, 28 for the album. After your first game with someone a parcel arrives: a board from Timosha's chess club, a set Sigurd carved from driftwood ("the queen looks like the postman"), pieces cast from Anselm's beeswax, a board of kaya wood from Kenji.

{% include figure.html path="blog/yourmove-post/shot-3.jpg" caption="Sigurd resigns, and sends a parcel." %}

Your own pieces remember. The knight from g1 is the same knight in every game; it counts its moves, captures, checks and forks, wears with use and earns a name: the Forker, the Crowned, the Mater. A win is a point, a draw half, and the points make titles by post, from Unrated to Correspondence master.

{% include figure.html path="blog/yourmove-post/shot-4.jpg" caption="Your set: the title, the board, and what each piece has done." %}

{% include figure.html path="blog/yourmove-post/shot-5.jpg" caption="The kitchen board: try it before you post." %}

## Good to know

**How strong do they play?**

Timosha has just learned the moves; Kenji plays like a strong amateur. After that their strength follows yours.

**Can the post go faster?**

Yes. In Settings choose express or pigeon post.

**It's the first version. What if something breaks?**

Please tell me: Settings has buttons to report a bug or suggest an idea through the StillCraft bot.

## Get it

- **Requires:** Android 9 (API 28) or newer.
- **Permissions:** notifications for arriving letters, optional. No internet permission at all.
- **License:** MIT. Fonts: Old Standard, Caveat and Manrope.

1. Download the APK on the phone.
1. Open it and allow installs from this source when Android asks.
1. Write how you sign your letters, choose the post and open the mailbox.

**[Download the APK](https://github.com/bigdevwhale/yourmove/releases/latest/download/YourMove.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/yourmove/) · [Source on GitHub](https://github.com/bigdevwhale/yourmove)

---

*Your Move is part of [Stillcraft](https://bigdevwhale.github.io/stillcraft/), a shelf of small, free, open-source apps for everyday life: no ads, no accounts, and your data stays on your phone. A new one appears here every few days.*
