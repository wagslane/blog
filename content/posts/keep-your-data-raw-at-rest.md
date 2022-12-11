---
title: Keep Your Data Raw at Rest
draft: false
date: 2021-06-30
description: Data at rest is more dangerous than data in motion.
author: Lane Wagner
tags:
 - clean-code
images:
 - /img/800/rest.avif.webp
videos: []
series: []
audio: []
---

I want to talk about a simple rule of thumb that has served me well over the years.

> If you have data that depends on other data, try not to store it.

If you follow this rule, you can deliver your code faster, mainly because you will avoid lengthy and painful data migrations.

The guiding principle behind this call to action is that *data at rest is more dangerous than data in motion*. By "data at rest" I typically mean data stored on disk, but it can also be any data that persists within your system through code updates and server restarts.

For example, in the context of a web application, the rows in a PostgreSQL database would be "at rest". Alternatively, when data is fetched from the database, transformed, and served we can say it’s "in motion" while in the application’s memory.

## Why is data dangerous at rest?

Data at rest is dangerous for several reasons.

* **It’s not easily versioned.** The database of your actively running web app might have some snapshots stored, but it’s not as incrementally versioned as your code. It should be less dangerous to update code than it is to manually muck with a database.
* **Its format isn’t easily changed.** When you decide to change the field of a database column, or when you decide to use a float instead of an int in ElasticSearch, it can be a significant pain to make those changes. You might need to write a custom script or do an expensive reindexing job. On the contrary, changing your code that deals with data in memory is typically easy and safe. Hopefully, it even has unit and integration tests.
* **Data has dependencies.** Your code is dependent on the data, not the other way around. As such, when you do update your data’s format, you need to synchronize that change with the code that depends on it. You’ll also probably need a rollback plan for the data in case something goes wrong.

## Why is data safer in memory?

Conventionally, data stored in memory, while your application is running, is ephemeral – you won’t depend on it being there for any length of time. Device and server restarts will wipe all in-memory data out, and that’s okay.

Since data being actively acted in by your code is ephemeral, we can make code changes with impunity. As long as the new version of the code is consistent with itself (and the dangerous persistent data it relies on) then changes are easy.

## When you have a piece of data that’s dependent on other data, do your best not to store it

Let’s say for example, that you’re building a movie review app. In your database, for each review, you store a field called `num_stars`, which is just an integer between 1 and 5. Then, later in development, the product team asks for a new feature that shows color on the review depending on the number of stars. 1 and 2 stars will be shown as red, 3 as yellow, and 4 and 5 are green. There are a couple of ways you can build this feature.

### Option 1 – Store the calculated field in the database

You could create a new field in your database called `color`. Whenever a review is created, you simply check the number of stars and based on that you generate the `color` field and store it. Now when a user pulls that review from the database it’s ready to go with the `color` field generated!

### Option 2 – Generate the calculated field every time it’s needed

Whenever a review is requested from the database, the application code pulls the review data, then generates the `color` by doing a [post-hoc](https://en.wikipedia.org/wiki/Post_hoc) calculation.

### Choose option 2 over option 1

Both options are probably equally easy to code and ship. The problems with option 1 will only show up later. Let’s look at a few potential issues.

* **We decide we want five colors.** Reviews with 2 stars will now be orange, while 4-star reviews will be a yellow-green mush. Because the color data is stored, we need to write a script to go update all the colors in the database based on the dependency (num_stars). Had we taken option 2, it would be as simple as updating and deploying some code.
* **A bug is deployed.** A new developer misunderstood the purpose of the code that generates and inserts the color field in the database. As such, they deployed a small bug that sets the color to always be green regardless of the num_stars field. Once the bug is discovered, again, we’ll need to write a, potentially dangerous, script or query to fix bad historical data. Had we used option 2, it would be a simple code fix.

## Is it ever a good idea to store something that’s directly dependent on other data?

As with all rules of thumb, there are exceptions.

For example, sometimes it’s necessary for performance reasons. Say you have some raw podcast audio data, and you want to be able to show users the transcript. It would be far too slow and expensive to re-transcribe the audio each time a user wants to look at the transcript. In cases like this, we need to just bite the bullet and store the calculated text data.
