---
layout: post
title: "Farwalker: a pedometer that walks you round the real world"
tags:
- android
- stillcraft
- walking
- maps
- open source
thumbnail_path: blog/farwalker-post/hero.jpg
---

A step counter tells you that you walked 7,412 steps today. Tomorrow it tells you 6,980. After a year you've walked something like two and a half thousand kilometres, and all you have to show for it is a bar chart. The distance is real; it just never goes anywhere.

So I made it go somewhere. Farwalker takes every step your phone counts and walks a little traveller across the real map of the world, along real roads, over real borders, to wherever you point them. It's free, with no ads and no account.

{% include figure.html path=page.thumbnail_path %}

**[Download the APK](https://github.com/bigdevwhale/farwalker/releases/latest/download/Farwalker.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/farwalker/) · [Source on GitHub](https://github.com/bigdevwhale/farwalker)

## What's inside

### The real world

The world comes from Natural Earth, the public-domain map data cartographers use: 5,580 towns, each with its own time zone, and the outlines of 195 states and 47 other lands. Between neighbouring towns run roads that keep to land, and every road knows where along it the borders are. Where the sea is in the way there are sea lanes, from short ferries to the long crossings: Lisbon to New York, Cape Town to Perth, Ushuaia to the Antarctic Peninsula. A handful of real bridges and tunnels are in there too, so you can walk from Copenhagen to Malmö or through the Channel Tunnel to Dover, but not across the Strait of Gibraltar.

### Steps move the traveller

The phone's own step counter does the counting, even with the app closed. Open it in the evening and the traveller catches up with your day: a road walked kilometre for kilometre, a town reached, maybe a border crossed. There are no streaks to keep and nothing to lose. A day at home is a day resting in town.

{% include figure.html path="blog/farwalker-post/shot-1.jpg" caption="The way so far, and the way ahead." %}

### Coins, trains and ships

Here's the thing about walking round the world: at six kilometres a day, Moscow to Lisbon takes more than two years. That's part of the charm, but I wanted a choice. So every kilometre you walk is also a coin. A coin buys eight kilometres by train, from anywhere on the road to a station ahead, or fifteen by sea. Trains and ships run on the real clock: the train covers seventy kilometres an hour, the ship twenty-eight, and Lisbon to New York is nine days at sea and about four hundred coins.

Walking never crosses an ocean. When the road meets the sea, the traveller waits in port, and your walks around the block pay for the passage.

### The passport

Every border crossed is a stamp: a circle, an oval, an octagon, in red or blue or violet ink, always the same for the same country, as if each border post had its own rubber stamp. The passport counts the states, the other lands, the seven parts of the world and the twenty-two ends of the earth: Cabo da Roca, Nordkapp, the Cape of Good Hope, Cape Horn, Cape Dezhnev, and, for the very patient, a trek across the ice from McMurdo to the South Pole.

{% include figure.html path="blog/farwalker-post/shot-3.jpg" caption="A stamp for every border." %}

### The engraved atlas

I wanted it to look like an old atlas rather than a navigation app. The seas are hatched in fine lines, three ripples run along every coast, the countries you've stamped get a hand-coloured wash inside their borders, and the one underfoot a red one. The road walked is a red line, a train ride the black-and-white railway of old maps, a sea crossing a dashed wake. Every name sits in a halo of paper so you can read it at a glance.

{% include figure.html path="blog/farwalker-post/shot-2.jpg" caption="Up close: the towns along the road." %}

### Where the traveller is

The panel under the map shows the road underfoot as a strip from one town to the next, today's kilometres and steps, the coins in the purse, and two small things I've grown fond of: the local time where the traveller stands, and the real weather there. Walking to the bakery on a grey Monday while somewhere in Castile it's twenty degrees and sunny is a strange little pleasure.

### The journal

Each night goes into the journal: which town, or which stretch of road between two, or on the train, or at sea, with the borders and ports of the day.

{% include figure.html path="blog/farwalker-post/shot-4.jpg" caption="Where each night was spent." %}

{% include figure.html path="blog/farwalker-post/shot-5.jpg" caption="Where to? Countries nearby and the ends of the earth." %}

## Good to know

**Does it count steps when the app is closed?**

Yes. The phone's step counter counts all the time, and the traveller catches up when you open the app. The only steps that can get lost are the ones between the last reading and a phone reboot.

**Isn't taking the train cheating?**

It's a choice. Walk every kilometre if you like, or save up for tickets and see more of the world. Every coin is still a kilometre you walked.

**How are the borders drawn?**

As Natural Earth draws them. The 195 states are the members and observers of the UN; the other lands with their own outline give a stamp too.

**What leaves the phone?**

Only the traveller's coordinates, for the weather where they are. Steps and the journey stay on the phone.

## Get it

- **Requires:** Android 9.0 (API 28) or newer, a phone with a step counter. No Google services needed.
- **Permissions:** physical activity, for the step counter; notifications, for news of the road; internet, for the weather.
- **License:** MIT.

1. Download the APK on your phone.
1. Open it and allow installs from this source when Android asks.
1. Pick your town, allow step counting and choose a goal.

**[Download the APK](https://github.com/bigdevwhale/farwalker/releases/latest/download/Farwalker.apk)** · [Stillcraft page](https://bigdevwhale.github.io/stillcraft/apps/farwalker/) · [Source on GitHub](https://github.com/bigdevwhale/farwalker)

---

*Farwalker is part of [Stillcraft](https://bigdevwhale.github.io/stillcraft/), a shelf of small, free, open-source apps for everyday life: no ads, no accounts, and your data stays on your phone. A new one appears here every few days.*
