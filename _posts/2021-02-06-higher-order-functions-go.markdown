---
layout: post
title:  "Go functions, closures and handlers"
date:   2021-02-06
categories: go
tags: go code
description: While Go is not a functional language, functions are a central part of Go. They can be assigned to variables, passed as parameters and returned from other functions. This is known as support for first class functions and it allows Go programmers to compose higher order functions and closures. Understanding these mechanisms can be a powerful addition to the tool belt of any programmer, so let's take some time to explore the behaviour of Go functions. Let's get functional and elegant!
---

{% include read_time.html %}

While Go is not a functional language, functions are a central part of Go. 
They can be assigned to variables, passed as parameters and returned from other functions. This is known as *support for first class functions* and it allows Go programmers to compose higher order functions and closures. 

Understanding these mechanisms can be a powerful addition to the tool belt of any programmer, so let's take some time to explore the behaviour of Go functions. 

**Let's get functional and elegant!**

{% include go_functions.html %}

## Basics
Functions are reusable code blocks, dedicated to performing well defined logic on inputs or *parameters* and producing outputs or *return values*.

In Go, functions are declared using the `func` keyword. The naming convention is to use `camelCase`. Unless exported, functions are available in the same package that they are declared into. 

```go
func myFirstFunction() {
    fmt.Println("This is my first function!")
}
```

Functions are allowed to take zero or more parameters and return zero or more outputs. The return types must be declared in the function signature.

The function `hiBye` below says hello and goodbye using 2 different return values. 

```go
func hiBye(name string) (string, string) {
	return fmt.Sprintf("Hello, %s!", name),
		fmt.Sprintf("Goodbye, %s!", name)
}

func main() {
	hi, bye := hiBye("favourite reader")
	fmt.Println(hi)
	fmt.Println(bye)
}
```

`main` prints out these values to produce the output below. See this in action on the <a href="https://play.golang.org/p/JB5bIV_gqFr" target="_blank">Go playground</a>.

```
Hello, favourite reader!
Goodbye, favourite reader!

Program exited.
```

Go also supports *anonymous functions*, which can be defined inline without having to be explicitly named. In the example below, `hiBye` now only lives inside `main`, but it will have the same output as before. See for yourself in the <a href="https://play.golang.org/p/7KW81yKG8SX" target="_blank">Go playground</a>.

```go
func main() {
	hiBye := func(name string) (string, string) {
		return fmt.Sprintf("Hello, %s!", name),
			fmt.Sprintf("Goodbye, %s!", name)
	}
	
	hi, bye := hiBye("favourite reader")
	
	fmt.Println(hi)
	fmt.Println(bye)
}
```

Functions can also be *deferred* using the `defer` keyword. The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns. The code below defers the call to `hiBye` until `main` finishes its work. 

```go
func main() {
	hiBye := func(name string) {
		fmt.Printf("Hello, %s!\n", name)
		fmt.Printf("Goodbye, %s!\n", name)
	}

	defer hiBye("favourite reader")

	fmt.Println("Main says hello and goodbye!")
}
```

Due to the deferred function call, `main` prints first, finishes, after which the greetings are printed. See this in action on the
<a href="https://play.golang.org/p/tABOudtvR2c" target="_blank">Go playground</a>.

```
Main says hello and goodbye!
Hello, favourite reader!
Goodbye, favourite reader!

Program exited.
```

## Closures
Closures are a special case of anonymous functions, which can be a little confusing for those not used to them, so they deserve a moment to investigate. 

A *closure* is a function value that references variables from outside its body. The function may access and assign to the referenced variables; in this sense the function is *bound* to the variables. 

The example below returns a closure, each referencing its own index and greet slice, thus allowing it to maintain state between invocations. 

```go
func greet(greetings []string) func() string {
	index := 0
	return func() string {
		greet := fmt.Sprintf("%s!", greetings[index])
		index++
		return greet	
	}
}

func main() {
	his := []string{"Hello", "Salut", "Hej"}
	byes := []string{"Goodbye", "La revedere", "Farvel"}
	hi, bye := greet(his), greet(byes)
	for i := 0; i < len(his); i++ {
		fmt.Println(
			hi(),
			bye(),
		)
	}
}
```

`main` prints out these values to produce the output below. See this in action on the
<a href="https://play.golang.org/p/XI1N_YcFZUT" target="_blank">Go playground</a>.

```
Hello! Goodbye!
Salut! La revedere!
Hej! Farvel!

Program exited.
```

## Higher order functions
As seen in the basics section above, Go functions can be passed around, returned and assigned to variables. This is particularly useful for constructing a function, but not immediately invoking it. 

*Higher order functions* are those functions composed from other functions. They can either accept a function as a type or return a function. Functions can be treated as any other value type, so composing higher order functions is easy and seamless. 

The `hiBye` function can now be composed of 2 separate `greet` functions and invoked only once.

```go
func print(name string, hi func(string) string, bye func(string) string) {
	fmt.Printf("%s\n%s\n", hi(name), bye(name))
}

func hiBye() (func(string) string, func(string) string) {
	hi := func(name string) string {
		return fmt.Sprintf("Hello, %s!", name)
	}
	bye := func(name string) string {
		return fmt.Sprintf("Goodbye, %s!", name)
	}
	return hi, bye
}

func main() {
	hi, bye := hiBye()
	print("favourite reader", hi, bye)
}
```

`main` prints out these values to produce the same output. See this in action on the
<a href="https://play.golang.org/p/Xank_7EAK8K" target="_blank">Go playground</a>.


```
Hello, favourite reader!
Goodbye, favourite reader!

Program exited.
```

## Handlers
An interesting notable mention when discussing functions is in the context of building HTTP servers using the <a href="https://golang.org/pkg/net/http/" target="_blank">`net/http` package</a>. 

An HTTP *handler* is a function that runs in response to a request made to an HTTP server.  
In Go, handlers are registered on server routes using the
<a href="https://golang.org/pkg/net/http/#ServeMux.HandleFunc" target="_blank">`http.HandleFunc` convenience function</a>. It sets up the default router in the `net/http` package and takes a function as an argument.

```go
func (mux *ServeMux) HandleFunc(pattern string, handler func(ResponseWriter, *Request))
```

A common way to write a handler is by using the
<a href="https://golang.org/pkg/net/http/#HandlerFunc" target="_blank">`http.HandlerFunc` adapter</a> on functions with the appropriate signature. Functions serving as handlers take a `http.ResponseWriter` and a `http.Request` as arguments. The response writer is used to fill in the HTTP response.

```go
type HandlerFunc func(ResponseWriter, *Request)
```
The `hiBye` function explored so far can be easily converted to a handler function to serve the greetings explored so far. Note how the `hiBye` function is passed to the `HandleFunc` as a parameter.

```go
func hiBye(w http.ResponseWriter, req *http.Request) {
	fmt.Fprintf(w, "Hello!\nGoodbye!\n")
}

func main() {
	http.HandleFunc("/hibye", hiBye)

	fmt.Println("Listening on :8090...")
	http.ListenAndServe(":8090", nil)
}
```

The full code can be seen in the
<a href="https://play.golang.org/p/1kCaXqrIdGi" target="_blank">Go playground</a>, but it will need to be run locally and tested using `curl`. The output should be as below: 

```
$ curl localhost:8090/hibye
Hello!
Goodbye!
```

The signature of the `HandlerFunc` is pre-defined, so what would happen if the function needed more parameters passed to it? As before, it would be great to pass a name to the greeting, as below:

```go
func hiBye(w http.ResponseWriter, req *http.Request, name string) {
	fmt.Fprintf(w, "Hello, %s!\nGoodbye, %s!\n", name, name)
}

func main() {
	http.HandleFunc("/hibye", hiBye("favourite reader"))

	fmt.Println("Listening on :8090...")
	http.ListenAndServe(":8090", nil)
}
```

This throws compilation errors because `HandleFunc` is expecting a function parameter, but now the `hiBye` function has been invoked with only one parameter. Oh dear, what a pickle! 

```
$ go run handlers.go
# command-line-arguments
./handlers.go:13:36: not enough arguments in call to hiBye
        have (string)
        want (http.ResponseWriter, *http.Request, string)
./handlers.go:13:36: hiBye("favourite reader") used as value 
```

While there are other solutions to this problem out in the wild, a common solution is to use a *wrapper function* or *closure* to pass the parameter to the `hiBye` function. In this way, the returned function still conforms to the expected signature and the greeting gets its extra parameter. 

```go
func hiBye(name string) http.HandlerFunc {
	return func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintf(w, "Hello, %s!\nGoodbye, %s!\n", name, name)
	}
}

func main() {
	http.HandleFunc("/hibye", hiBye("favourite reader"))

	fmt.Println("Listening on :8090...")
	http.ListenAndServe(":8090", nil)
}
```

The full code can be seen in the
<a href="https://play.golang.org/p/7nTl51S9tfK" target="_blank">Go playground</a>, but it will need to be run locally and tested using `curl`. The output should be as below: 

```
$ curl localhost:8090/hibye
Hello, favourite reader!
Goodbye, favourite reader!
```

## Parting words
That's it for now. I've covered a lot of ground, but a few key points to remember are: 
- Go functions can be assigned to variables, passed as parameters and returned from other functions
- Functions can be anonymous. This means they are declared inline and are not explicitly named
- Closures are anonymous functions that are bound to variables outside of their body 
- Higher order functions are composed from other functions. They can either accept functions as a type or return functions
- Handlers are functions that run in response to a request made to an HTTP server

Having a good understanding of functions can help solve a wide variety of problems through composition. I hope this post gave you an insight into the wonderful world of Go functions.

**Happy Go coding!**
