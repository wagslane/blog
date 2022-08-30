---
title: Func-y JSON, an alternative to REST
draft: false
date: 2022-05-17
description: REST, GraphQL and gRPC aren't what I want
author: Lane Wagner
tags:
 - clean-code
images:
 - /img/800/funky.avif.webp
videos: []
series: []
audio: []
---

It's insane to me that almost every web developer in the world is working with web APIs, and yet the developer experience remains atrocious. Most applications I've worked on use a RESTful scheme, a GraphQL implementation, or a more strict gRPC system. I've used REST for the most part, probably just because it's what I'm familiar with, but I really hate it. Every time I design a new API I can't help but think that it shouldn't be so hard to come up with a sane way to communicate between the server and client.

First, before we dive into my *probably terrible* proposal, let's review some encumbant API design frameworks, after all, I'm a big believer in [checking for existing standards](https://blog.boot.dev/clean-code/use-existing-standards/) before creating your own.

## REST

Like I said, I use REST a lot. I like that it doesn't require any special software to be installed, most developers are vaguely familiar with it, and works on top of a standard HTTP 1 connection. Most servers and clients assume you'll be using making standard HTTP requests, so configuration of infrastructure is generally a simple task.

The biggest problem I have with REST is that it requires *so much* documentation and architecture design. There are many ways to accomplish the same thing. Are parameters passed as headers, path parameters, or query parameters? Are things like authentication requests really `POST`s if they don't create anything?

To sum up the problem with REST, *there are too many ways to do the same thing, requiring a lengthy API design and documentation process*.

## GraphQL

GraphQL improves upon some of the issues REST has. I would argue the biggest benefits GraphQL offers include a self-documenting API and the ability for the client to specify which data they want returned in any given request. I love these improvements, but I still have some complaints.

First, GraphQL isn't a simple "idea" like REST that you can easily build from scratch. GraphQL's methodology is quite heavy-handed, so you're almost *forced* into using a third-party library that enforces all the protocols. Second, the promise that the client can ask for data in "whatever way they want" is only partially true. The server still needs to support all the nested queries that the client could ask for in order for them to work, and more importantly, in order for them to work efficiently.

## gRPC

So far, gRPC is my favorite option. Remote procedure calls allow us to essentially make function calls over a network connection, which is the best developer experience I can imagine. There are only 3 big downsides to gRPC that I'm worried about, and they are the only things that keep me from using it more.

First, gRPC runs on HTTP 2.0. As a result, getting two servers to talk to each other and properly and load-balance requests can take some configuration. Most infrastructure defaults don't assume you'll be doing gRPC. I like to think this will get better in the future, but it's still a very real pain point. Second, gRPC doesn't really work in a browser client unless you use some [fairly hacky techniques](https://grpc.io/docs/platforms/web/quickstart/). Additionally, if you're building an API for external users, it is *not* safe to assume your users will be comfortable using a gRPC API.

## My solution: "Func-y JSON"

First, I'm not talking about [json:api](https://jsonapi.org/). I've looked into that, and it's not what I'm interested in. Here are my goals with `Func-y JSON`:

1. Make API interactions feel like standard function calls
2. Allow HTTP 1, don't force HTTP 2+
3. Keep it simple and extensible. You won't need special libraries on the server or client side.
4. [Anti-bikeshedding](https://en.wiktionary.org/wiki/bikeshedding) by only allowing one method for the API designer to receive function inputs.
5. Throw down the shackles of "resource thinking". Authentication isn't *really* a CRUD operation, for example.

To steal from ["The Zen of Python"](https://peps.python.org/pep-0020/):

> Simple is better than complex.
> There should be one-- and preferably only one --obvious way to do it.
> Namespaces are one honking great idea -- let's do more of those!

Let's go over how "Func-y JSON" works.

### The API is made up solely of HTTP requests and responses with JSON bodies

* The [Content-Type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type) header is always `application/json`.
* Other headers *may* be used, but should *not* affect the core logic of the function. For example, [[Content-Length](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Length)].
* Request and response bodies are always valid JSON payloads.

### We think of that API like an in-code API. It's just "exported" functions.

* All "function inputs" are passed into the JSON request body
* All "function outputs" are returned in the JSON response body

### We always use POST requests

We don't have HTTP methods on function calls, so we shouldn't have them here.

### We always use 200 response codes

We don't have response codes on function calls, so we shouldn't have them here. Specify errors in the body.

### All functions are versioned

The first part of any request's path is a semantic version. For example, `/v1.0/...`

### Functions can have 0 to many namespaces. Namespace(s) follow the version number

Unlike REST, these namespaces aren't *necessarily* resources, it's just an organizational tool.

For example, `/v1.0/stats/...`

### The function name is the last part of the path

For example, `/v1.0/stats/private/get_num_payments`

### The entire path, and all function input/output keys are snake_case

Snake case seems like the best option because *I like it and I'm writing this blog post.* Also, URLs are not case sensitive, so it's not a dangerous choice.

## Example documentation

Now that you understand the simple rules, let's look at some example docs.

### Get number of payments: `/v1.0/stats/private/get_num_payments`

#### Inputs

```
{
    "filter_by_card_provider": *String
}
```

#### Outputs

```
{
    "error": *String,
    "count": *Number
}
```

### Authenticate function: `/v1/auth`

#### Inputs

```
{
    "user_id": String,
    "password: String
}
```

#### Outputs

```
{
    "error": *String,
    "token": *String
}
```

## A caveat: Don't actually use this

I literally just wrote this up today because I'm bored. Don't get me wrong, at the time of writing I'm digging on this idea. That said, I'm sure if anyone actually reads this they'll tell me all sorts of things I've overlooked.
