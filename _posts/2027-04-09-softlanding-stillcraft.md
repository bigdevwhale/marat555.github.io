---
layout: post
title: "Soft Landing: a calorie counter where every day is a balloon flight"
tags:
- android
- stillcraft
- nutrition
- gamification
- open source
thumbnail_path: blog/softlanding-post/hero.jpg
---

Every calorie counter I tried ended the same way. A week of careful logging, then a birthday, a day over the limit painted red, and the quiet feeling that the streak was ruined anyway. The other problem was simpler: logging took too long. Search, scroll, pick the right "buckwheat" out of forty, type the grams, repeat for every item on the plate.

So Soft Landing does two things. It makes logging fast: barcodes, label photos, a whole meal typed in one line. And it turns the day into something you'd actually like to finish well: a hot-air balloon flight to the next village. It's free, with no ads and no account.

{% include figure.html path=page.thumbnail_path %}

**[Download the APK](https://github.com/bigdevwhale/softlanding/releases/latest/download/SoftLanding.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/softlanding/) · [Source on GitHub](https://github.com/bigdevwhale/softlanding)

## What's inside

### The flight

In the morning the balloon is light and high. Everything you eat is ballast, and the balloon comes down. The aim is to land on the meadow by evening: your norm, give or take. Too much and you come down early in a farmer's field. Too little and the wind carries you past the meadow. The meadow is a corridor on purpose: starving yourself is a miss too, not a win.

The day card says it plainly: how many calories are left, a strip with what's eaten and where the meadow is, and one line like "500–710 kcal more and the landing is soft".

### Fast to log

There are two big buttons on the main screen, a scanner and "Add food". The scanner is Google's own, so the app needs no camera permission, and it reads ordinary barcodes as well as the Russian «Честный знак» DataMatrix codes. An unknown pack is looked up in Open Food Facts and then kept on the phone.

The search box also takes a whole meal: "buckwheat 200, 2 eggs, cappuccino" becomes three entries and one button. Above the search sit "same as yesterday" and the foods you usually have for this meal, each with a "+" that adds it at the amount you had last time.

{% include figure.html path="blog/softlanding-post/shot-1.jpg" caption="Scan, label, calories, dish, or one line." %}

### When a food isn't there

Photograph the label. The nutrition table is read on the phone, and the numbers are checked against each other: protein×4 + fat×9 + carbs×4 has to come close to the calories. That check also rescues Russian labels, which the on-device reader sees through Latin eyes ("белки" comes out as "6enkn"): if the words are garbled, the app looks for three numbers in a row that add up.

{% include figure.html path="blog/softlanding-post/shot-2.jpg" caption="One number, one row of units, one button." %}

### Dishes from the pot

Soup is where calorie counters usually give up. Here you put in what goes into the pot, scan the packs straight into it, and weigh the finished pot once, with the pot's own weight taken off. Every serving after that is counted right, because the water that boiled off and the grain that swelled are in the weight. A dish can be passed to another phone as a QR code.

{% include figure.html path="blog/softlanding-post/shot-6.jpg" caption="Weigh the pot once." %}

### The route

Each soft landing moves you to the next village. There are 31 of them, in the Meadowland, the Lakeland and the Foothills, up to Cloud Harbour, where balloons moor to towers like ships. Every village has a line about it and a simple recipe, and the recipe goes into your dishes in one tap, already counted. A rough day doesn't send you back: the balloon just spends the night in the fields.

{% include figure.html path="blog/softlanding-post/shot-3.jpg" caption="A village per soft landing." %}

{% include figure.html path="blog/softlanding-post/shot-4.jpg" caption="And its recipe." %}

### Fairs and fog

Two days a month can be fairs: a birthday, a holiday. Nothing is counted, and the run keeps going. A day with nothing written down is fog, an empty page in the logbook, not a failure.

### The logbook and the hangar

The logbook keeps a month of coloured landings, the week in numbers, your weight as a smoothed line towards its goal, and a handwritten note from the aeronaut for every day: "Came down in the oats short of the village. The farmer's wife grumbled and asked us to supper." The hangar holds twelve balloon envelopes that open with soft landings.

{% include figure.html path="blog/softlanding-post/shot-5.jpg" caption="A month of landings." %}

## Good to know

**How is the norm worked out?**

Mifflin–St Jeor with your activity and the pace you choose, never below a safe floor, with a protein target. If a doctor or a coach gave you a number, it replaces the formula.

**What leaves the phone?**

Only barcodes and search words, to Open Food Facts, and that can be switched off. Everything else stays on the phone.

**What if I eat late at night?**

The day starts at 4 am, so a midnight snack still belongs to yesterday.

## Get it

- **Requires:** Android 9.0 (API 28) or newer. Scanning and label reading use Google Play services.
- **Permissions:** internet, for Open Food Facts; notifications, for one evening note if the day is still almost empty. No camera permission.
- **License:** MIT.

1. Download the APK on your phone.
1. Open it and allow installs from this source when Android asks.
1. Tell it about yourself once, then press «Add food» at breakfast.

**[Download the APK](https://github.com/bigdevwhale/softlanding/releases/latest/download/SoftLanding.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/softlanding/) · [Source on GitHub](https://github.com/bigdevwhale/softlanding)

---

*Soft Landing is part of [Stillcraft](https://bigdevwhale.github.io/stillcraft/), a shelf of small, free, open-source apps for everyday life: no ads, no accounts, and your data stays on your phone. A new one appears here every few days.*
