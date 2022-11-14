---
layout: post
title:  "The Future of Code Week: Will Languages like Go and Rust Overtake their Legacy?"
date:   2022-11-14
categories: jobs
tags: go career interviews
description: In this post, I share some of my key thoughts from my LinkedIn Learning Future of Code Week episode discussing emerging languages. This session takes place on November 17th 2022. I was thrilled to be invited to share my thoughts with Marcus and Ray. 
---
In this post, I share some of my key thoughts from my LinkedIn Learning Future of Code Week episode discussing emerging languages. This session takes place on November 17th 2022. I was thrilled to be invited to share my thoughts with Marcus and Ray. 

**Make sure to sign up to  <a href="https://bit.ly/foc-ep04" target="_blank">the event on LinkedIn!</a>**

{% include read_time.html %}

<div class="container">
    <div class="row">
      <figure class="centered">
        <a href="https://bit.ly/foc-ep04" target="_blank">
                <img class="centered" src="{{site.baseurl}}/assets/foc_thumbnail.png" alt="Talk banner">
            </a>
        </figure>
    </div>
</div>

## Session summary
<blockquote>
Emerging languages often try to change how developers solve problems, but they also often change the course of legacy languages. Programming languages like Go and Rust are not just inventing new ways to work with code, but also having a clear effect on new versions of existing languages. Will the new take over the old? Will the adjustments to existing programming languages be enough to stave innovation? Or, is it best at some point to start from scratch and rethink the foundation of how we approach programming?
</blockquote>

## What Go does well
Go has been hailed as a language that is **easy to use and deploy**. There are a few reasons that should be top of mind when discussing this: 
- **Small syntax** - Go's syntax has been made small by design, so that it's easy to pick up and become productive with. One of the <a href="https://golang.org/doc/faq#creating_a_new_language" target="_blank">purposes of the Go project</a> was to simplify the act of programming and make it easier for developers to write statically typed code.
- **Static typing** - Go code is strongly, statically typed and compiled. The compiler catches a variety of bugs and allows for certain compile time performance optimizations. Go has built-in types for slices & maps which are easy to use.
- **Structs** - Go isn't technically <a href="https://golang.org/doc/faq#Is_Go_an_object-oriented_language" target="_blank">an object oriented language</a>, but custom types are created using the `struct` keyword. Structs can have methods attached to them in the same way objects do. They have been described as *lightweight objects* since they do not support type inheritance.
- **Interfaces** - While Go doesn't support type hierarchy, `interface` types create reusable behaviours in a versatile and robust way according to the principle of <a href="https://en.wikipedia.org/wiki/Composition_over_inheritance" target="_blank">composition over inheritance</a>. Interfaces are *implicitly implemented* on structs by the Go compiler. A struct can implement several interfaces just as long as it implements all the methods that they require. A common use case of interfaces is to wrap code coming from an external library into an interface on the side of *the calling code* and only expose a subset of external functionality in the internal codebase.`interface{}` is known as *the empty interface*. It is satisfied by all types and is used as a parameter type by code that handles values of unknown type.
- **Building and testing Go code** - The <a href="https://golang.org/cmd/go/" target="_blank">go command</a> exposes functionality for building & testing Go code. This is easy to use and comes with the default Go installation. The source code is built into a binary executable. Executables can be built for other targets/architectures using *cross compilation*.
- **Deploying Go code** - The binary executable of Go source code contains not just application code, but also all the support code needed to execute the binary. This means that the Go executable can run without the need for system dependencies such as Go tooling to run on a new system, unlike other languages. Go executables can be *directly deployed*.
- **Generics** - recently added to Go, generics allows us to write better libraries that and data structures. From my experience, adoption of generics in production has been cautious. The Go team have put together <a href="https://go.dev/blog/when-generics" target="_blank">talks and posts</a> which discuss when *not* to use generics, as they are not a silver bullet to better code. 

## Performance & Concurrency 
Go is a **fast and performant** language due to some very important optimizations:
- **Inlining** - the Go compiler saves function calls by treating the body of a function as if it were part of the calling function. This has the trade off of increasing binary size, so Go only does so when it makes sense.
- **Escape analysis** - Go implements this optimization which analyzes the scope of references to a value. If the references do not escape scope then they are saved on the *stack* which is much faster and does not need to be garbage collected.
- **Garbage collection** - prevents memory leaks and has very low latency. The Go garbage collector is an intricate piece of code that you can check out on <a href="https://tip.golang.org/doc/gc-guide" target="_blank">the Go Blog</a>.
- **Concurrency** - Goroutines are multiplexed to a small number of OS threads. They are described as *lightweight threads* with small stack sizes that are *cooperatively scheduled* and managed by the Go runtime. Goroutines yield control at natural stopping points, making them more performant than kernel managed threads.

One of the big advantages of using Go is the **easier and more intuitive model of concurrency** that is central to the language:
- **Goroutines** - Functions/methods can be run concurrently to other functions/methods by starting a Goroutine using the `go` keyword. They allow our application to use its resources optimally and perform more than one piece of work. All of the complex management and scheduling of goroutines is abstracted from the developer. Built on the concept of coroutines that is also used in Kotlin.
- **Channels** - Goroutines communicate through channels, which can be thought of as pipes through which data flows. Channels avoid data races by design as they maintain queues of goroutines and data themselves.The solid basics of Goroutines and channels can be combined to form a variety of basic and advanced concurrency patterns. Two notable concurrency patterns are <a href="https://blog.golang.org/pipelines" target="_blank">pipelines</a> and <a href="https://golangbot.com/buffered-channels-worker-pools/" target="_blank">worker pools</a>.

## The future of Go
In my opinion, the future of Go is bright ☀️: 
- **Replacing legacy services** - due to its superior performance, engineering teams will continue to decompose monoliths and write new services in Go. In an increasing amount of companies, Go and microservices go together hand in hand. This de-facto choice is because Go's
<a href="https://pkg.go.dev/net/http" target="_blank">net/http package</a> makes writing REST APIs easy.
- **Increased demand** - Go has recently "become a teenager" with its 13th birthday. It usage has been increasing and I would argue that it is moving away from emerging language status, as it has become adopted by major tech companies for mission critical jobs. You can see <a href="https://go.dev/solutions/#case-studies" target="_blank">the full list of case studies on the Go blog</a>.
- **Compatibility promise** - <a href="https://twitter.com/_rsc" target="_blank">Russ Cox</a> gave a <a href="https://youtu.be/v24wrd3RwGo" target="_blank">talk at GopherCon 2022</a> about Go's compatibility promise, which I really recommend. When updating to a new version of Go, you must compile your code again, but the team ensure that new versions do not break existing code. These guarantees make Go suited for writing Go code for the long term.

<blockquote>
"Go is boring, and that's good!" - Russ Cox
</blockquote>

<div class="container">
    <div class="row">
        <figure class="centered">
            <a href="https://youtu.be/v24wrd3RwGo" target="_blank">
                <img class="centered" src="{{site.baseurl}}/assets/boring_go.png" alt="GopherCon 2022: Russ Cox - Compatibility: How Go Programs Keep Working">
            </a>
        </figure>
    </div>
</div>

## Parting words
Obviously, I'm a huge Go-lover and I believe that the language is here to stay. It's got so many cool features the Go team do a great job to improve tooling and add new features. An often underrated aspect is **community**. I'm proud to be part of the Go community, which is inclusive and welcoming. I think Gophers worldwide will continue to advocate for the language and grow.

Join the <a href="https://gophers.slack.com/" target="_blank">Gophers Slack</a>.