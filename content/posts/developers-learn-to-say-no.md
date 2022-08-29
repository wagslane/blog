---
title: "Learn to Say 'No'"
author: Lane Wagner
date: "2022-06-27"
categories: 
  - "news"
images:
  - /img/say-no.webp
---

Saying "no" is hard skill to learn. It's even harder if you tend to be a more introverted person, but learn how to say "no" effectively can help your career. I certainly have struggled over the years with saying "no" as a programmer, after all, wouldn't a *good programmer*{{< superscript "tm" >}} be able to do *anything*?

Let's look at some example scenarios where perhaps you should be saying "no" more often.

## Say "no" to product managers

The single biggest struggle when product and engineering teams are collaborating is the disconnect that happens when creating a product roadmap.

In theory, it goes like this:

1. Product team knows how much business value feature X will bring to the company: (A).
2. Engineering team knows how long feature X will take to release: (B).
3. The two parties talk it over, perform an `A/B` calculation, and the product team decides whether or not building the feature is a worthwhile investment of resources.

There are several things that can go wrong here, but the least obvious, and perhaps most dangerous, is that **engineer's egos are attached to the estimates**. As a result, estimates are often too short, and business decisions are made using less-than-ideal data. This can be exacerbated by the fact that product manager's egos are attached to their new feature ideas, meaning that "A" in the A/B calculation is larger than it should be while "B" is smaller than it should be. The end result is features being built that should have been identified earlier as bad investments.

When an eager product manager is telling you that a cool new feature will double the companies revenues, it can be hard to have the guts to tell them the truth about how long you think it will take to build. You may be tempted to tell them it can be done faster than you think. You'll just work extra hard, maybe even on weekends, in order to deliver on time, right?

**Please don't shorten your estimates due to business pressures.**

It's better for everyone if your estimates are as close to the mark as they can be, because that will result in the best business decisions being made. Hell, if your company culture is a toxic one, you might even need to *pad* your estimates. While exaggerating estimates isn't the best way to run a product organization, if you have a management team that doesn't know how to manage well, it can be necessary to protect yourself and your team.

While trying to be helpful it can be very tempting to over-promise, but the best developers give the most realistic timelines they can so they can be confident about their ability to deliver.

## Say "no" to requests for an estimate

I've been pushed before by product teams to "provide a deadline" on new features or products. If you can give an accurate estimate, say `+/- 25%`, then go for it. However, I've found myself in situations where the task could take anywhere from 1 week to an entire quarter. In fact, after some preliminary research, I might even discover the proposed solution is overly complicated and realize the entire project is a poor business decision altogether.

Don't be afraid to push back when you feel you don't have enough information to provide a decent estimate. Simply ask for some time to look into more technical details first. For example, you could say "I'm not sure how long it will take to build that new chatbot, but I should be able to give you an estimate by Friday".

## Say "no" to functionality that belongs in another team's codebase

At larger companies, it's often the case the different teams manage different services and pieces of the companies IT infrastructure. When a new feature needs to be added, the product team may not know which engineering team(s) should be working on it. As the engineers get together to discuss, again, it can be tempting to volunteer for the feature, maybe your team even has extra bandwidth and the other teams are swamped! It's okay to step in and help, but only if doing so also happens to result in a sane technical architecture. Let's look at an example.

* Team A is in charge of the "outgoing emails" service
* Your team, team B, is in charge of the "users" service
* Team A is super busy, and product needs new email sequence features

If team "A" weren't so busy, they would be building the new email sequences, but they *are* busy, so you decide to volunteer your team - after all, the "users" service is stable and your team has the bandwidth for the work. Additionally, you decide that your team doesn't have the domain expertise to make changes to the "outgoing emails" service, so you'll just add some new dependencies and get the "users" service sending those email sequences.

The problem with this reasoning is that it creates a sub-par technical architecture. You made a bad technical decision in order to speed things up in the short term. Almost certainly, sometime in the future, someone is going to have to move all that logic back into the part of the system that it belongs in for one reason or another.

## Say "no" to application-specific requests in your shared libraries

I think it is *really good* code architecture practice for developers to maintain some kind of shared library. When you only work on application code, it can be hard to understand why it's important to separate concerns properly. For example, if your React webapp needs to make API calls to a backend server, why not just do an inline [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)?

After working on a codebase that is shared by many applications, you will begin to have a better understanding of why it's important to keep application-specific concerns out of shared libraries.

Let's pretend you're building a simple password validation library. It exposes a function called `getPasswordComplexity(String) -> Int`. It takes a `string` as input, and returns a "complexity score" from `0-100`. Some of your users might ask you to also return specific error messages when the "complexity score" is below a certain threshold. Something like:

> Please enter a longer password

The problem is, each application will likely have *different* thresholds! Additionally, it's a near-certainty that even if you write fairly generic error messages, many applications will want their own custom text.

The answer to these kinds of requests should be a simple "no, that's a concern of your application code".

## Know when to say "yes"

Finally, it's worth mentioning that it's important to stay cognizant of the times it's important to say "yes". Saying no *too much* can turn you into an unhelpful asshole. No one wants that. I have only a couple pieces of advice here:

* Say no kindly, don't condescend
* Always explain why you are saying no
* Be willing to change your mind when presented with a good argument
* Try to offer alternative solutions. "I don't think we should do X, but Y might accomplish the same goal and make more sense"

Good luck!
