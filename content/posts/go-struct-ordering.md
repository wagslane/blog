---
title: Saving a Third of Our Memory by Re-ordering Go Struct Fields
draft: false
date: 2020-08-07
description: Simply changing the order of some uint variables we managed to drop our memory usage significantly
author: Lane Wagner
tags:
 - golang
images:
 - /img/800/tetris.jpg.webp
videos: []
series: []
audio: []
---

We had application at one of my previous companies that typically ran with ~2GB in memory at any given time, but simply changing the order of some uint variables we managed to drop the memory usage to less than 1.4GB. Let’s dive into how inefficient field ordering in Go structs can have a huge impact on the memory footprint of a program.

## Our Situation

The vast majority of the allocated memory in our case was due to an enormous slice of `stats` structs. The `stats` struct had the following type definition:

```golang
type stats struct {
	NumPosts uint8
	Reach    uint16
	NumLikes uint8
}
```

In theory, this struct will use a measly 4 bytes. Each `uint8` uses 1 byte, and the single `uint16` needs 2. We began to suspect that we had some wasteful memory issues, so I built the following little program to show how memory is being used by our struct:

```golang
package main

import (
	"fmt"
	"reflect"
	"runtime"
)

type stats struct {
	NumPosts uint8
	Reach    uint16
	NumLikes uint8
}

func main() {
	typ := reflect.TypeOf(stats{})
	fmt.Printf("Struct is %d bytes long\n", typ.Size())
	n := typ.NumField()
	for i := 0; i < n; i++ {
		field := typ.Field(i)
		fmt.Printf("%s at offset %v, size=%d, align=%d\n",
			field.Name, field.Offset, field.Type.Size(),
			field.Type.Align())
	}

	allStats := []stats{}
	for i := 0; i < 100000000; i++ {
		allStats = append(allStats, stats{})
	}

	printMemUsage()
}

func printMemUsage() {
	var m runtime.MemStats
	runtime.ReadMemStats(&m)
	fmt.Printf("Alloc = %v MiB", bToMb(m.Alloc))
	fmt.Printf("\tTotalAlloc = %v MiB", bToMb(m.TotalAlloc))
	fmt.Printf("\tSys = %v MiB", bToMb(m.Sys))
	fmt.Printf("\tNumGC = %v\n", m.NumGC)
}

func bToMb(b uint64) uint64 {
	return b / 1024 / 1024
}
```

On my MacBook using Go 1.14.1 the above program prints:

```
Struct is 6 bytes long
NumPosts at offset 0, size=1, align=1
Reach at offset 2, size=2, align=2
NumLikes at offset 4, size=1, align=1
Alloc = 1084 MiB        TotalAlloc = 3012 MiB   Sys = 2713 MiB  NumGC = 19
```

Notice that even though `NumPosts` only has a size of 1 byte, the next field, `Reach`, still starts at offset 2. A whole byte is being wasted! The same thing happens with the `NumLikes` field, it starts at offset 4 with a size of 1, but the struct still takes up the full 6 bytes.

This may not seem like a big deal, but when you are storing millions of these structs in memory the bloat starts to add up quick.

If we change the stats struct such that the `uint16` isn’t defined between the `uint8`s:

```golang
type stats struct {
	Reach    uint16
	NumPosts uint8
	NumLikes uint8
}
```

```
Struct is 4 bytes long
Reach at offset 0, size=2, align=2
NumPosts at offset 2, size=1, align=1
NumLikes at offset 3, size=1, align=1
Alloc = 694 MiB TotalAlloc = 1927 MiB   Sys = 1391 MiB  NumGC = 19
```

The total allocated memory drops from 3 GB to less than 2, and each instance of the struct now only uses 4 bytes.

## Why?

Modern CPU hardware performs reads and writes to memory most efficiently when the data is [naturally aligned](https://en.wikipedia.org/wiki/Data_structure_alignment). The memory that is stored side by side should be accessible using a common multiple, so the Go compiler makes sure that it is.

With our first struct, the Reach field is between the NumPosts and NumLikes fields, which means that the compiler will add some padding to keep things nice and even.

![mem1](/img/800/memory-usage-go.png.webp)

In our updated struct however, we have grouped the smaller fields, and since they add up to the same amount of memory as the larger Reach field we can save some space!

![mem1](/img/800/memory-usage-go-2.png.webp)

This was is a weird quirk, but making the small optimizations has made a huge impact on some of our services.
