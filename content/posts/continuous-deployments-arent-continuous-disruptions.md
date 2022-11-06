---
title: Continuous Deployments != Continuous Disruptions
draft: false
date: 2021-04-12
description: Data at rest is more dangerous than data in motion.
author: Lane Wagner
tags:
 - clean-code
images:
 - /img/800/loop.avif.webp
videos: []
series: []
audio: []
---

Luckily, I’ve met very few engineers in my career who are outright opposed to continuous deployment. That said, I have met some, and I think their hesitation is usually rooted in the myth that continuous deployment causes more disruptions to end-users than a batched release cycle.

## What is continuous deployment?

Before we get into the nitty-gritty of why continuous deployment (CD) is almost always better than batched releases, let’s define some terms. [Atlassian’s definition](https://www.atlassian.com/continuous-delivery/continuous-deployment) of CD is close, but I’m going to modify it a bit for my purposes. Unlike them, I don’t think CD requires continuous integration testing, but why you wouldn’t run tests is beyond me.

> Continuous Deployment (CD) is a software release process that deploys new versions to a production environment when certain conditions in a code repository are met.
> 
> -- Lane

For example, I work a lot in [Kubernetes](https://kubernetes.io/), so my CD often consists of a [Github Actions](https://github.com/features/actions) script that runs when I merge code into the main branch. The script fires off a Helm chart that updates deployments on the cluster.

There are many ways to set up a good CD pipeline, but the commonality is that when a trigger happens (code is merged or a tag is created) a new deployment is shipped to production automatically. This makes code deployments as painless as possible for the developer. With ideal continuous deployment, shipping a new update requires no more work than just writing the update.

## Doesn’t continuous deployment cause more bugs?

No.

[Accelerate](https://itrevolution.com/book/accelerate/) is a book that I highly recommend that uses real-world case studies to show the positive impact of modern DevOps practices. In the book, they compare high-performing software organizations to low-performing organizations. According to their data, high-performing companies:

* Deploy code ~46x more often to production
* Have a 440x faster commit to deploy time
* Have a 5x lower change failure rate ("failures" are "result[s] in degraded service or subsequently require remediation (e.g., leads to service impairment or outage, require a hotfix, a rollback, a fix-forward, or a patch.)
* Have a 170x faster mean recovery time after failures

While the data speaks for itself, it also makes sense from a qualitative perspective. Having systems in place that automate code delivery to be less time-consuming won’t inherently create more bugs. It’s the process before the CD that’s responsible for catching bugs. We can have a whole separate discussion about continuous integration and how that reduces the number of bugs that get through to production, but the act of automating deploys is orthogonal to that point.

Continuous deployments don’t create more bugs, but they do make it easier to get fixes out for existing bugs.

## Even without bugs, aren’t continuous updates disruptive to end users?

They can be, but it depends.

## Example 1: DotA 2 and other online games

I’m a fan of the popular PC game DotA 2. As a user of their game, I’m really annoyed by how often they release small patch updates. It’s not uncommon for me to play three or four games back-to-back and have to download updates in between games. This is a highly disruptive experience because, in order to update, I have to close the game client, download the patch, and restart the game.

The problem with what DotA 2 is doing isn’t that they (might) do CD, it’s that they have not made the patching experience seamless. For example, they could allow patches to be downloaded while the game is open like Blizzard does with their launcher. Alternatively, they could flag certain kinds of updates to be "lazy". For example, inconsequential bug fixes wouldn’t require a game restart could just update the next time you close the game.

## Example 2: Major UI updates

Another common complaint is that with continuous deployment, users will be constantly frustrated that the features and layout of the application are changing from underneath them.

This isn’t a problem with continuous deployment, it’s a problem with of product planning.

Just because continuous deployment gives us the tools to deploy UI/UX changes with less friction, doesn’t mean we should be doing it all the time. Sure, it’s probably not a big deal to change the wording on tooltips or make a shade of red a bit darker, but deploying wild menu and layout changes too frequently is an awful idea.

Figure out the rate of change your customers are okay with and build your roadmap around that. Again, this has nothing to do with your deployment processes.

There is only an upside to continuous deployment within the context of UI/UX. When you discover a typo in some explainer text you can have the fix up within minutes, no one wants to manually build, ssh, pull, stop and start a server for something so trivial.

## Example 3: Backend web servers

When I’m updating code on a backend server I can deploy that project asynchronously with no effect on the end user. There’s no reason not to use continuous deployment in such environments as long as the changes are backward compatible.

Backward compatibility is crucial in a continuous deployment process where multiple projects have asynchronous release cycles. It’s meaningless to point out how backward-incompatible changes to a REST API will break existing clients. Those same issues could be caused without continuous deployment.
