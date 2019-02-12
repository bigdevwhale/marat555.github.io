---
layout: post
title: "Simple Telegram bot in Laravel"
tags:
- Telegram
- Laravel
- Bot
- Channel
thumbnail_path: blog/thumbs/telegram-bot.png
add_to_popular_list: true
---

{% include figure.html path="blog/thumbs/telegram-bot.png" %}

**[Humor](https://t.me/humor_quotes)** - Humor quotes channel

**[Success](https://t.me/success_quotes_channel)** - Success quotes channel

**[Inspiration](https://t.me/inspiration_quote)** - Inspiration quotes channel

**[William Shakespeare](https://t.me/william_shakespeare_quotes)** - William Shakespeare quotes channel

**[George R.R. Martin](https://t.me/george_rr_martin_quotes)** - George R.R. Martin quotes channel

**[J.K. Rowling](https://t.me/jk_rowling_quotes)** - J.K. Rowling quotes channel

**[Stephen King](https://t.me/stephen_king_quotes)** - Stephen King quotes channel

**[J.R.R. Tolkien](https://t.me/jrr_tolkien_quotes)** - J.R.R. Tolkien quotes channel

**[TolkienQuotesBot](https://telegram.me/TolkienQuotesBot)** - J.R.R. Tolkien quotes bot (send a random quote in reponse to your message)

## Background
Recently I decided to write a simple bot for get some experience and the subsequent implementation of more complex projects. 
The choice fell on the bot of quotes for specific authors / books / genres / tags. 

To implement this idea, it was necessary to perform two basic steps:
1. Get a large amount of authors, books, quotes, tags data and structure them correctly in the form of tables in the database
2. Implement directly the server part of the bot, which would process all requests to the bot


## Data

I parsed over 5,000,000 web pages and reveived over 900,000 quotes from one well-known internet resource. 

For these purposes, I wrote a small Java application that processes the web page and records the necessary information in csv file.

The structure of the csv is as follows

| Quote | Author | Book    | Category| Tags |
|-------|--------|---------|---------|---------|
|  |  |  |

After a couple days of continuous work of this application, I received a sufficient amount of information that it was 
possible to record this information in the database. I did not save quotes directly to the database as at that time I 
hadn’t bought the server yet. =)

To handle csv, I created a simple laravel command class.

Database structure:

{% include figure.html path="blog/telegram-bot/diagram.png" %}

## Implementation

For a start, I bought a virtual server, a domain, generated an SSL certificate. 

Telegram API provides two mutually exclusive ways of getting bot updates:

* Webhooks
* Polling via getUpdates method

I think, the first way is more scalable because webhook requests can be loadbalanced between unlimited instances of your bot apps.
So I decided to use webhooks.

To implement telegram bots on Laravel I used [Telegram Bot API PHP SDK](https://github.com/irazasyed/telegram-bot-sdk) from GitHub.

First, I created a bot that sent a random quote to J.R.R. Tolkien books once an hour and in response to any message from the user.

**[TolkienQuotesBot](https://telegram.me/TolkienQuotesBot)** - here it is. 

But then I decided to transfer the functionality of hourly sending a message to the channel **([jrr_tolkien_quotes](https://t.me/jrr_tolkien_quotes))** in order to reduce the number of requests from my server.
I did this simply by adding a bot to the telegram channel and giving it administrator rights, and in the code I just send messages to this channel every hour.


## Channels

By analogy, I created the following channels:

**[Humor](https://t.me/humor_quotes)** - Humor quotes

**[Success](https://t.me/success_quotes_channel)** - Success quotes

**[Inspiration](https://t.me/inspiration_quote)** - Inspiration quotes

**[William Shakespeare](https://t.me/william_shakespeare_quotes)** - William Shakespeare quotes

**[George R.R. Martin](https://t.me/george_rr_martin_quotes)** - George R.R. Martin quotes

**[J.K. Rowling](https://t.me/jk_rowling_quotes)** - J.K. Rowling quotes

**[Stephen King](https://t.me/stephen_king_quotes)** - Stephen King quotes

**[J.R.R. Tolkien](https://t.me/jrr_tolkien_quotes)** - J.R.R. Tolkien quotes



## Summary

In this post, I described what it took me to create a simple telegram bots and channels.


