---
title: Thoughts on the "Guard" Proposal for Go's Error Handling
draft: false
date: 2022-11-05
author: Lane Wagner
tags:
 - golang
images:
 - /img/800/gopher.png.webp
imageAlts:
 - "Art by stable diffusion. Prompt: 'Gopher running and falling photo 4k'"
---

I found [this proposal](https://github.com/golang/go/issues/31442) for improvements to error handling in Go interesting, but still not something I'd be happy to see implemented.

Allow me to clear up my thoughts on Go's errors. Overall, I *prefer* how Go forces me to think about errors at every turn. When working in try/catch languages like JavaScript, I often easily forget which functions can throw. Even if I do remember, it's easy to think "I think this gets caught somewhere up the call chain". It's easy to ignore the fact that *things can go wrong*. I almost never have issues with unhandled errors in Go, and I love that.

Okay, so what *don't* I like? Well like everyone else, I think 3 extra lines after most function calls just to do the same old error handling can bloat a codebase real quick. The [classic example](https://go.googlesource.com/proposal/+/master/design/go2draft-error-handling-overview.md) looks like this.

*The simple CopyFile function:*

```go
func CopyFile(src, dst string) throws error {
	r := os.Open(src)
	defer r.Close()

	w := os.Create(dst)
	io.Copy(w, r)
	w.Close()
}
```

*The actual CopyFile function:*

```go
func CopyFile(src, dst string) error {
	r, err := os.Open(src)
	if err != nil {
		return err
	}
	defer r.Close()

	w, err := os.Create(dst)
	if err != nil {
		return err
	}
	defer w.Close()

	if _, err := io.Copy(w, r); err != nil {
		return err
	}
	if err := w.Close(); err != nil {
		return err
	}
}
```

## The "guard/must" proposal

```go
func CopyFile(src, dst string) (err error) {
	defer func() {
		if err != nil {
			err = fmt.Errorf("copy %s to %s: %v", src, dst, err)
		}
	}()

	r := guard os.Open(src) 
	defer must r.Close()		

	w := guard os.Create(dst)
	defer must w.Close()

	err = io.Copy(w, r)
	
	// here we need to do extra stuff when an Copy error happens: now we must use the 'normal' error handling method, and cannot use guard or must
	if err != nil { 
		_ := os.Remove(dst) // fail silently if errors happen during error handling
	}
	return
}
```

I'm going to be honest, I don't think this "guard/must" proposal is a big improvement in terms of practicality. The basic idea here is that:

* The `guard` keyword is syntactic sugar to return from a function when a function call returns in an error
* The `must` keyword is syntactic sugar to `panic` on any error returned by a function call

## Problem #1 - We don't need syntactic sugar for panic

[Panicking in Go is a code smell](https://go.dev/doc/effective_go#panic). We shouldn't be doing it very often, and when we do, I'm okay with it being a bit of a pain to write.

## Problem #2 - Missing context

Strictly returning errors in Go isn't always the best way to handle them, in fact it's usually not. It's best to [wrap them](https://blog.boot.dev/golang/wrapping-errors-in-go-how-to-handle-nested-errors/) with additional context. For example:

```go
func CopyFile(src, dst string) error {
    r, err := os.Open(src)
    if err != nil {
        return fmt.Errorf("could not copy file: %w", err)
    }
    ...
}
```

This proposal uses a `defer` function to add context to errors, which has several problems:

* Any lines of code salvaged by the `guard` keyword are lost by the new deferred func
* It's not simple, clear, or easy to understand
* The context added to all the errors in the function must be identical

## What's a better solution?

I love the idea of a `guard` keyword. I think it fits nicely within the pattern of [guard clauses in Go](https://blog.boot.dev/clean-code/guard-clauses/).

What if, instead, we forget the `must` keyword, and the `guard` keyword simply operates on a single error expression? Here's what that could look like:

```go
func CopyFile(src, dst string) error {
	r, err := os.Open(src)
	guard err
	defer r.Close()

	w, err := os.Create(dst)
	guard fmt.Guardf("could not create file to copy: %w", err)
	defer w.Close()

	guard io.Copy(w, r)
	guard w.Close()
}
```

### Notes

1. In the case that the `CopyFile` returns more than just a single error, `guard` would return zero values for everything but the non-nil error.
2. I made up the `fmt.Guardf` function. We would need *something* like this. It's a version of `fmt.Errorf` that returns `nil` if the error that's passed in is `nil`.

I think *something like this* could solve the only real problem with Go errors: How do we save those 3 lines of code while still encouraging good error wrapping?
