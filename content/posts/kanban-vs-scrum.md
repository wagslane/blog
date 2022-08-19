---
title: Kanban vs Scrum – Why Kanban is More Agile
draft: false
date: 2021-01-05
description: Let's discuss why I generally prefer Kanban to Scrum
author: Lane Wagner
tags:
 - management
images: []
videos: []
series: []
audio: []
---

Agile development is fantastic and has made software development more fun and productive while simultaneously less punitive and slow. While I'm a fan of Agile, I'm not a huge fan of one of it's more common interpretations - a Scrum-based workflow. Let's discuss why I generally prefer a "Kanban" workflow to traditional Scrum. If you’ve read my [first article](/posts/leave-scrum-to-rugby), you’ll know I’m not Scrum’s biggest fan.

## What is Agile Development?

From the [Agile Manifesto](https://agilemanifesto.org/):

1. Individuals and interactions over processes and tools.
2. Working software over comprehensive documentation.
3. Customer collaboration over contract negotiation.
4. Responding to change over following a plan.

The same authors of the manifesto also have provided the following [12 principles](https://agilemanifesto.org/principles.html) that back it up:

1. Our highest priority is to satisfy the customer through early and continuous delivery of valuable software.
2. Welcome changing requirements, even late in development. Agile processes harness change for the customer’s competitive advantage.
3. Deliver working software frequently, from a couple of weeks to a couple of months, with a preference to the shorter timescale.
4. Business people and developers must work together daily throughout the project.
5. Build projects around motivated individuals. Give them the environment and support they need, and trust them to get the job done.
6. The most efficient and effective method of conveying information to and within a development team is face-to-face conversation.
7. Working software is the primary measure of progress.
8. Agile processes promote sustainable development. The sponsors, developers, and users should be able to maintain a constant pace indefinitely.
9. Continuous attention to technical excellence and good design enhances agility.
10. Simplicity–the art of maximizing the amount of work not done–is essential.
11. The best architectures, requirements, and designs emerge from self-organizing teams.
12. At regular intervals, the team reflects on how to become more effective, then tunes and adjusts its behavior accordingly.

Assuming you agree with the majority of these principles, the question is, how do we put them into practice?

## Kanban vs Scrum

Kanban and scrum are two of the most popular strategies for putting Agile principles into practice. In my geographic area and perhaps generally, it seems Scrum is, unfortunately, more popular. I don't think it's because Scrum is better, but because it makes *managers feel better*. I’ve already pointed out most of my problems with [Scrum here](/posts/leave-scrum-to-rugby), so instead, in this article, I’ll try to focus on why I believe Kanban methods are more Agile. That said, I think every organization will have different needs depending on culture, preferences, size, etc.

Here are some quick definitions of Scrum and Kanban, but if you’re not familiar with the systems you may want to read up on them in more detail as well.

### Scrum

From the [Scrum guide](https://scrumguides.org/scrum-guide.html):

> A framework within which people can address complex adaptive problems, while productively and creatively delivering products of the highest possible value. In a nutshell, Scrum requires a Scrum Master to foster an environment where:
> 
> 1. A Product Owner orders the work for a complex problem into a Product Backlog.
> 
> 2. The Scrum Team turns a selection of the work into an Increment of value during a Sprint.
> 
> 3. The Scrum Team and its stakeholders inspect the results and adjust for the next Sprint.
> 
> 4. Repeat

### Kanban

> Kanban is a lean method to manage and improve work across human systems. This approach aims to manage work by balancing demands with available capacity, and by improving the handling of system-level bottlenecks. 
> 
> Work items are visualized to give participants a view of progress and process, from start to finish—usually via a [Kanban board](https://en.wikipedia.org/wiki/Kanban_board). Work is pulled as capacity permits, rather than work being pushed into the process when requested.

## Comparing the two

Atlassian has a [great comparison chart](https://www.atlassian.com/agile/kanban/kanban-vs-scrum):

|                     | Scrum                                             | Kanban                        |
| ------------------- | ------------------------------------------------- | ----------------------------- |
| Cadence             | Regular fixed-length sprints (ie, 2 weeks)        | Continuous flow               |
|                     |
| Release methodology | At the end of each sprint                         | Continuous delivery           |
|                     |
| Roles               | Product owner, scrum master, the development team | No required roles             |
|                     |
| Key metrics         | Velocity                                          | Lead time, cycle time, WIP    |
|                     |
| Change philosophy   | Teams should not make changes during the sprint.  | Change can happen at any time |
|                     |

At first glance, Kanban at the very least *appears* more Agile, but let’s go over some critical points in more detail.

## Continuous Flow

As I explain in my criticism of Scrum, Scrum inserts artificial stopping points in the middle of the development lifecycle. The purpose of the sprints is to remind the team to go back and get feedback from customers at the end of each cycle and use that feedback as input to the next cycle.

While feedback is of utmost importance, I’ve found that few teams actually go talk to customers in-between sprints. The underlying incorrect assumption is that getting customer feedback can (or should) be scheduled on a regular cadence. Instead, Kanban takes a more realistic approach and solicits customer feedback constantly without adding stopping points to slow everything down.

In Kanban, we create a task, and we’re always getting customer feedback. The instant the feedback indicates that it’s time to pivot, we just pivot. Sometimes that means throwing away a task or changing its direction completely. That’s okay. Most importantly, we can just get our tasks done, we don’t have to break them down artificially, have meetings to talk about burndowns, or pretend we can estimate their duration using t-shirt sizes.

## Continuous Delivery

Kanban places emphasis on continuous integration and delivery. If you’re a fan of DevOps as much as I am, this point is critical. There are many arguments for delivering software as quickly as possible, but here are the key points:

* **Increased ability to compete in the market.** If a competitor can only release features and bug fixes every two weeks, their products will not only improve at a slower pace but will appear less stable. With good CI/CD practices, many bugs can be fixed the same-day.
* **Code is deployed sooner and starts making money faster.** We write code to improve products and make money. The longer code is complete but waits sitting in a queue the more money we’re wasting.
* **Engineers are happier and more productive.** Engineers like to see the code they’ve worked on be deployed and get used by people. With long release cycles, it takes longer to get that dopamine hit. Easy and frequent deployments to production make happy engineers, and happy engineers stick around.
* **Higher code quality.** If we think of each deployment as the code going through the car wash then it makes sense that more frequent and easy deploys will keep the codebase cleaner. I have a couple of small refactors I want to get in, is it worth it if I have to wait two weeks to get them merged in? By then I will likely have a merge conflict or two.

## No required roles

Scrum has a product owner, a project manager (dubbed the scrum master), and a development team. Kanban has team members. There actually isn’t much to say here other than what I've already said about [hands-off project managers](https://wagslane.dev/posts/managers-that-cant-code/) being a mistake.

My main criticism of the way many Scrum teams are organized is that the person who is responsible for timelines often doesn’t have the product knowledge or technical chops to understand what's going on. They should know why we are working on a certain feature or bug next and ideally even have the technical knowledge to understand why all of this coding stuff is taking so long.

## Estimates

When I work, I don’t take estimates as seriously as most Scrum teams do. I’m not sure if this is actually a Kanban vs Scrum idea – but I'll give you my personal 2 cents.

Estimates are a necessary component of running a business. We need to have an idea of how soon we can have a feature in customer's hands. If we’ve concluded a feature can add $150,000 of revenue each year, that may be great if it takes 2 weeks to get it out the door, but is a horrible time investment if it will take 2 years to complete.

My opinions with estimates are the following:

* They aren’t deadlines. No one should be getting punished for not meeting an estimate.
* Ideally, managers and team leads are capable of estimating tasks they assign with minimal meetings and disruptions to the rest of the team. Again, this requires hands-on managers.
* Estimates should be made in real-time units. A task is estimated to take 1, 2, or 4 weeks, not L, XL, or XXL t-shirt sizes.
