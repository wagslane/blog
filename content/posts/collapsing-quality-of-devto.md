---
title: The Collapsing Quality of Dev.to
draft: false
date: 2021-09-13
description: Why is it getting so much worse?
author: Lane Wagner
tags:
 - writing
images:
 - /img/800/photo-1571686683059-4b1bf2086a13.avif.webp
videos: []
series: []
audio: []
---

A few years ago I found Dev.to, and was delighted by the writing experience. It felt like a platform with all the conveniences of Medium's writing tools but wasn't missing the features that developers need, like code blocks and markdown. I still think Dev.to probably has the best writing experience of any third-party blogging platform for devs, luckily that hasn't changed at all. On the other hand, the reading experience, or rather the content on the platform, has dropped so hard and fast in quality that I realized a few months ago that I haven't opened my feed in almost a year. I used to browse articles on Dev.to before checking Reddit and Twitter, but over the last twelve months, my lizard brain realized it never found anything on Dev.to, and I subconsciously just stopped going there.

## What made Dev.to great in the first place?

Dev.to launched in 2015 (I believe, this was surprisingly difficult to search for). And I personally noticed it sometime in 2018. As an amateur writer looking for new places to publish my content, and as an avid reader of programming articles, I was excited to discover the platform. Here were the features that stuck out to me initially:

* Auto publish to Dev.to from an external RSS feed
* Write in markdown, with support for code syntax blocks
* Dark mode
* Welcoming community
* Good content for beginners
* Authors own their own content (per the terms and agreements)

Dev.to has always seemed to be geared toward beginners, and there’s nothing wrong with that. In fact, the internet desperately needed a place for new developers to hang out without being flamed off of platforms like Reddit and StackOverflow, or overwhelmed by the more advanced topics on HackerNews.

## What went wrong?

Building a great platform is hard, but building an engaging and worthwhile community on that platform is much harder. There is a constant yin/yang relationship between content [moderation and censorship needs](https://blog.boot.dev/misc/online-misinformation-and-censorship/), whether most of the moderation decisions are made by the platform itself or are crowdsourced by the users. Dev.to did an amazing thing by focusing its branding and growth strategy around newer developers. **The content on Dev.to was almost exclusively entry-level JavaScript content, by beginners, for beginners.**

It all went south when the community reached a point where writers realized that it’s easy to write low-effort content that will be liked by brand-new programmers, even if you’re brand new yourself. I’m not saying new devs shouldn’t be writing, I absolutely think they should. It’s that the return on investment of writing clickbaity listicles on Dev.to is much better than writing something new, interesting, or insightful.

As I’m writing this article, my Dev.to feed has the following four articles at the top:

* Top 5 IDE for Web Development – 67 likes
* Using WebSockets with React – 6 likes
* You don’t know spread operator – 11 likes
* Map, Filter, and Reduce explained – 6 likes

I’m definitely judging these articles by their titles, and maybe they actually contain great content, but my experience on Dev.to recently has left me jaded. Did we really need another article that has likely plagiarized the Google SERP for `map`, `filter`, and `reduce` usage? Does an article that lists five popular code editors deserve the attention it’s getting? My experience says probably not.

## Why is it a hard problem to solve?

I’m sure the Dev.to team has realized this is a problem, so my guess is it’s a hard one to solve. Let’s try to figure out what makes the solution so elusive.

In my experience, HackerNews and /r/programming are the two best communities, at least of the larger ones, for readers to find quality programming content. The issue is that the content found there isn’t written for beginners. Dev.to made a place for newbies to write and read, but the content is getting worse and worse.

There is an important point to be made that to an experienced developer, almost all entry-level content could seem low-effort and low-quality. Let’s do our best not to fall into that trap. There is nothing inherently wrong with an article about the `map`, `filter`, and `reduce` functions, but my suspicion is that this particular article is just a worse version of what you’d find on the first page of Google.

Sure enough, as I now click through to the body of "Map, Filter and Reduce explained", I see code blocks without syntax highlighting enabled, sentences without a space between the last period and the first letter of the next sentence, and a slew of grammatical mistakes that could be solved with a simple writing plugin. It’s just not high-quality stuff. This article was clearly written very quickly, without much effort. Why is it at the top of my feed?

It’s easy to get experienced developers to downvote, flag, or even outright ban low-effort content. All the articles I listed above, e.g. "Top IDE for Web Development" would never have seen the light of day on HackerNews. I’m convinced that the underlying problem is that a platform for beginners will have a smaller ratio of knowledgeable community members who have the experience required to identify poor or incorrect explanations.

## What could Dev.to do better?

I think several changes could help. Dev.to has taken queues from blogging and social sites like Medium and Facebook, in that it doesn’t allow downvotes or flagging of content. This is likely done to encourage a positive and inclusive community, which is obviously a fantastic goal. The side effect is that garbage content lives on.

1. Add the ability to downvote or flag content as low-quality, but without allowing other readers to see those flags. It would give the platform a way to get the good stuff to the top, but wouldn’t make newer writers feel picked on in public. It’s worth noting that [Dev.to currently has something like this](https://dev.to/community-moderation), but only trusted users can take those actions, and it’s clearly not enough.
2. In addition to the flagging, it would be nice to be able to leave constructive feedback, without embarrassing the author, and without the community labeling you a hater. In other words, I’d like to be able to leave a note on a specific part of the article, explaining privately to the author what they could do to improve the content.
3. Foster a community for experienced developers. You don’t want the blind leading the blind. You still need a healthy amount of knowledgeable people in your community to help the newer folks. Keep in mind, you don’t actually need much-advanced content to keep them around, but they’re leaving in droves because their contributions can’t compete with the clickbait.
4. Better automatic article imports. A lot of my articles that were imported via RSS had some janky post-import artifacts. My guess is this stuff could be mitigated by simple detection algorithms. For example, automatically detecting the language of code blocks, adding the first image in the post as the featured one and not including it twice, or notifying the writer of difficulties parsing the RSS feed, so crap doesn’t get published accidentally.

## I like the goal of Dev.to

I really hope they make some changes, whether they be the ones I’ve suggested or even better ideas that the Dev team comes up with on their own. In the meantime, I won’t be reading anything on the platform because it’s just not interesting, and I also believe it’s unhelpful to new programmers. If you work for Dev.to and want to chat with me about my thoughts, feel free to reach out.
