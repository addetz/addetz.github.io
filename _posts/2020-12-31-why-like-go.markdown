---
layout: post
title:  "Why do you like Go?"
date:   2020-12-31
author: Adelina Simion
categories: go
description: I was asked this question - "Why do you like Go?" - during several job interviews. The question itself is not tough, but it is broad and full of opportunities to trip up. It's difficult to distill your thoughts into a coherent answer in a high pressure situation. I'm going to use this post to nail down an answer to this dreaded question.
---
{% include read_time.html %}

I was asked this question - "Why do you like Go?" - during several job interviews. 
The question itself is not tough, but it is broad and full of opportunities to trip up. It's difficult to distill your thoughts into a coherent answer in a high pressure situation. 

I'm going to use this post to nail down an answer to this dreaded question.

**Let's Go!**

{% include go_cactus.html %}

## What the question means
The question is ambiguous, but it has been posed to me in this form. Even though I write Go code daily, I wouldn't say I have any personal feelings of "like" towards it. 

The question is meant to be a conversation starter about the usage and strenghts of Go.
In my experience, the answer should touch upon the broad areas presented below. 

## Writing & deploying Go code
Go has been hailed as a language that is **easy to use and deploy**. There are a few reasons that should be top of mind when discussing this: 
- **Small syntax** - Go's syntax has been made small by design, so that it's easy to pick up and become productive with. One of the [purposes of the Go project](https://golang.org/doc/faq#creating_a_new_language) was to simplify the act of programming and make it easier for developers to write statically typed code
- **Static typing** - Go code is strongly, statically typed and compiled. The compiler catches a variety of bugs and allows for certain compile time performance optimizations. Go has built-in types for slices & maps which are easy to use 
- **Structs** - Go isn't technically [an object oriented language](https://golang.org/doc/faq#Is_Go_an_object-oriented_language), but custom types are created using the `struct` keyword. Structs can have methods attached to them in the same way objects do. They have been described as *lightweight objects* since they do not support type inheritance
- **Interfaces** - While Go doesn't support type hierarchy, `interface` types create reusable behaviours in a versatile and robust way according to the principle of [composition over inheritance](https://en.wikipedia.org/wiki/Composition_over_inheritance). Interfaces are *implicitly implemented* on structs by the Go compiler. A struct can implement several interfaces just as long as it implements all the methods that they require. A common use case of interfaces is to wrap code coming from an external library into an interface on the side of *the calling code* and only expose a subset of external functionality in the internal codebase.`interface{}` is known as *the empty interface*. It is satisfied by all types and is used as a parameter type by code that handles values of unknown type
- **First class functions** - Go functions can be assigned to variables, passed as parameters to other functions and returned from other functions. This is known as *support for first class functions*. This allows Go programmers to compose higher order functions and closures, which are particularly useful for *dynamic programming*
- **Building and testing Go code** - The [`go` command](https://golang.org/cmd/go/) exposes functionality for building & testing Go code. This is easy to use and comes with the default Go installation. The source code is built into a binary executable. Executables can be built for other targets/architectures using *cross compilation*
- **Deploying Go code** - The binary executable of Go source code contains not just application code, but also all the support code needed to execute the binary. This means that the Go executable can run without the need for system dependencies such as Go tooling to run on a new system, unlike other languages. Go executables can be *directly deployed*

## Performance of Go
Go is a **fast and performant** language due to some very important optimizations:
- **Inlining** - the Go compiler saves function calls by treating the body of a function as if it were part of the calling function. This has the trade off of increasing binary size, so Go only does so when it makes sense
- **Escape analysis** - Go implements this optimization which analyzes the scope of references to a value. If the references do not escape scope then they are saved on the *stack* which is much faster and does not need to be garbage collected
- **Garbage collection** - prevents memory leaks and has very low latency
- **Concurrency** - Goroutines are multiplexed to a small number of OS threads. They are described as *lightweight threads* with small stack sizes that are *cooperatively scheduled* and managed by the Go runtime. Goroutines yield control at natural stopping points, making them more performant than kernel managed threads

## Concurrency in Go
One of the big advantages of using Go is the **easier and more intuitive model of concurrency** that is central to the language:
- **Goroutines** - Functions/methods can be run concurrently to other functions/methods by starting a Goroutine using the `go` keyword. They allow our application to use its resources optimally and perform more than one piece of work. All of the complex management and scheduling of goroutines is abstracted from the developer
- **Channels** - Goroutines communicate through channels, which can be thought of as pipes through which data flows. Channels avoid data races by design as they maintain queues of goroutines and data themselves
- **Concurrency patterns** - The solid basics of Goroutines and channels can be combined to form a variety of basic and advanced concurrency patterns. Two notable concurrency patterns are [pipelines](https://blog.golang.org/pipelines) and [worker pools](https://golangbot.com/buffered-channels-worker-pools/)

## Parting words
Since the answer to this broad question had a lot to cover, I have only given some brief headline answers to each topic. In an interview setting, you should be prepared to be asked to delve into more detail on any topics of particular interest. I am not be able to provide this level of detail in this post, but I have left some further reading links below which should be useful.

Go is particularly suited for building high volume, scalable, performant web applications. It has been [gaining lots of popularity in the Dev community](https://insights.stackoverflow.com/survey/2020#technology-most-loved-dreaded-and-wanted-languages-loved) and has been adopted by big players such as Google, Uber and Netflix. I hope this blog post will help you answer questions about the benefits and strengths of Go, whether in an interview or not. 

**Happy Go coding!**

## Further reading

[Interfaces](https://golangbot.com/interfaces-part-1/)

[First class functions](https://golangbot.com/first-class-functions/)

[Five things that make Go fast](https://dave.cheney.net/2014/06/07/five-things-that-make-go-fast)

[Getting to Go: The Journey of Go's Garbage Collector](https://blog.golang.org/ismmkeynote)

[Introduction to Concurrency](https://golangbot.com/concurrency/)

[Writing Web Applications](https://golang.org/doc/articles/wiki/)

