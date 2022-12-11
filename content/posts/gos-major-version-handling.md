---
title: Go’s Major Versioning Sucks – From a Fanboy
draft: false
date: 2020-09-15
description: I'm normally a fan of the rigidity within the Go toolchain
author: Lane Wagner
tags:
 - golang
images:
 - /img/800/handling.avif.webp
videos: []
series: []
audio: []
---

I'm normally a fan of the opinionated rigidity within the Go toolchain. In fact, we use Go on the [front and backend at Boot.dev](https://boot.dev), and we've found that it's wonderful to have standardized formatting, vetting, and testing across the entire Go ecosystem. The first big criticism I've had with Go's opinionated nature is with the way the Go toolchain handles major versions. It slows down development in a significant number of scenarios and is a detriment to the average developer's experience.

## Refresher on "go mod"

Go modules, and the associated commands `go mod` and `go get` can be thought of as Go's equivalents to NPM and Yarn. The Go toolchain provides a way to manage dependencies and lock the versions that a collection of code (a module) depends on.

One of the most common operations is to update a dependency in an existing module. For example, my typical workflow goes something like this:

```bash
# update all dependencies
go get -u ./...

# add missing dependencies and remove unused ones
go mod tidy

# save all dependency code in the local "vendor" folder
go mod vendor
```

## Semantic versioning

Go modules use [Git tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging) and [semantic versioning](https://semver.org/) to keep track of the versions of dependencies that are compatible with the module in question. Semantic versioning is a way to format version numbers and it looks like this: `v{MAJOR}.{MINOR}.{PATCH}`. For example, `v1.2.3`.

Each number is to be incremented according to the following standards:

```
MAJOR version when you make incompatible API changes,
MINOR version when you add functionality in a backwards compatible manner, and
PATCH version when you make backwards compatible bug fixes.
```

So far so good, I love everything about Go's dependency management up to this point. My favorite part is that the Go toolchain doesn't have a central repository of builds you need to publish to to share a package. We just use Git repositories! Amazing.

## The big problem: major version increments

The Go decided that all versions beyond `v0` and `v1` are required to use the major version in the module path. Keep in mind, this is redundant because the local `go.mod` file already has the semantic version of all dependencies tracked. 

There are two ways to accomplish this path requirement.

### The first and recommended way

> To start development on `v2` of `github.com/googleapis/gax-go`, we'll create a new `v2/` directory and copy our package into it.
>
> -- [The Go blog](https://blog.golang.org/v2-go-modules#TOC_4.)

In other words, for every major version, we are encouraged to maintain a new copy of the entire codebase. This is also the only way to do it if you want pre-modules users to be able to use your package. I understand why for large projects this makes a ton of sense, it allows the maintainers to continue patching old versions easily while developing the new version.

### The second way

The second option is to change the name of your module in `go.mod`. Fore example, `module github.com/wagslane/go-tinydate` would become `module github.com/wagslane/go-tinydate/v2`. Besides this not working for older versions of Go, I also find it problematic because it breaks (in my mind) one of the most useful things about module names - they reflect the file path. I tend to choose the first option as recommended.

### Why does this suck?

* **It's hardly intuitive.** Newcomers to the Go ecosystem are going to have to learn this weird convention that's *almost completely* enforced by the toolchain. I found myself staring at my command line in confusion when trying to get to a next major version until I went and read through all the documentation.
* **Users of packages aren't alerted about new major versions** Because the import path is completely different, when users type `go get -u`, expecting to get the latest version of a package, they won't even get a command-line warning that a new major version exists. I understand not auto-updating, but at least tell me that I'm behind the curve.
* **For the client to update, it's not a simple path change in go.mod** Users actually need to `grep` through their codebase and change each import statement to point to the new major version, it's a terrible developer experience.
* **It's overkill for small or internal packages.** I work on a team that maintains many microservices, and we have a couple of packages that are internal to our org but are shared by various projects. Due to our size, we don't need any kind of backward compatibility, we just update everything. It's quite annoying to have overbearing rules when we simply don't care.

## Some ideas for the way I wish it were

* Simply remove the path requirement. Maintainers should be able to increment the major version via Git tags.
* Update the `go get` command to continue not auto-updating major versions, but to give a warning message that a new major version exists.
* Add an optional flag to `go get` to allow for updating major versions.

This all comes down to a fundamental issue I have with the "import compatibility rule".

> If an old package and a new package have the same import path, the new package must be backwards compatible with the old package.
> 
> [Import compatibility rule](https://research.swtch.com/vgo-import)

I agree with the sentiment that we should only increment major versions when making breaking changes, but more often than not breaking changes are **really easy** to accommodate. Go is a strongly typed language, and almost all breaking changes will cause compiler errors that are simple to fix. This kind of rule would add a lot more value to a language like Python or JavaScript.

### A Caveat - Diamond Imports

Using different paths for different major versions makes more sense in situations where we may require two different versions of the same package, you know, [diamond imports](https://research.swtch.com/vgo-import#dependency_story) and all that. This is the exception, not the rule, and it seems strange to tap dance around a problem that doesn't exist in most codebases.

## Why this is annoying for me personally

I often want to build a package that has domain-specific logic that will only be used only in microservices at the small company I work for. For example, we have a repo that holds the `struct{}` definitions for common entities used across our system. Occasionally we need to make backward-incompatible changes to those struct definitions. If it were an open-source library we wouldn't make changes so often, but because it's internal and we are aware of all the dependencies, we change the names of exported fields _regularly_. We aren't changing names because we chose bad ones, to begin with, we are usually changing names because requirements from the product side change rapidly in a startup.

This means major version changes are a fairly regular occurrence. Some say that we should just stay on `v0`, and that's a reasonable solution. The problem is these ARE production packages that are being used by a wide number of services. We want to Semver.

Go makes updating major versions so cumbersome that in the majority of cases, we have opted to just increment minor versions when we should increment major versions. We want to follow the proper versioning scheme, we just don't want to add unnecessary steps to our dev process.

## I get why these rules exist, and I think they are great for large open projects

I understand why these decisions were made - and I even think in a lot of cases they were great decisions. For any open-source or public facing module the rules make great sense. The Go toolchain is enforcing strict rules that encourage good API design. In their effort to make public APIs great, they made it unnecessarily hard to have good "local" package design.

There is an [open issue on Github](https://github.com/golang/go/issues/40323) that would make new major versions more discoverable from the CLI. Take a look at that if you are interested.

Go still has the best toolchain and ecosystem. NPM and PIP can suck it.

If you disagree, [@ me on Twitter](https://twitter.com/wagslane).
