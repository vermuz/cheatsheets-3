---
layout: page
title: Go
permalink: /go/
---

## Summary

* Imperative language
* Statically typed
* Syntax similar to Java/C/C++, but less parantheses and no semicolons
* Compiles to native code (no JVM)
* No classes, but structs with methods
* Interfaces
* No implementation inheritance. There's type embedding, though.
* Functions are first class citizens
* Functions can return multiple values
* Go has closures
* Pointers, but not pointer arithmetic
* Built-in concurrency primitives: Goroutines and Channels

## Quick Start

1. Create Directories

    ```bash
    mkdir $HOME/Go
    mkdir -p $HOME/Go/src/github.com/username
    ```
2. Setup Paths

    ```bash
    //  add to ~/.bash_profile
    export GOPATH="$HOME/Go"
    export PATH="/usr/local/bin:$PATH:$GOPATH/bin"
    ```

3. Install Go

    ```bash
    brew install go
    ```

4. "go get" the basics

    ```
    go get golang.org/x/tools/cmd/godoc
    go get golang.org/x/tools/cmd/vet
    ```

## Basic Syntax

File `hello.go`:

```go
package main

// fmt - short for format
import "fmt"

func main() {
    fmt.Println("Hello World!")
}
```

```bash
go run hello.go
```

##Functions

```go
// a simple function
func functionName() {}

// function with parameters (again, types go after identifiers)
func functionName(param1 string, param2 int) {}

// multiple parameters of the same type
func functionName(param1, param2 int) {}

// return type declaration
func functionName() int {
    return 42
}

// Can return multiple values at once
func returnMulti() (int, string) {
    return 42, "foobar"
}
var x, str = returnMulti()

// Return multiple named results simply by return
func returnMulti2() (n int, s string) {
    n = 42
    s = "foobar"
    // n and s will be returned
    return
}
var x, str = returnMulti2()
```

##Functions As Values and Closures

```go
func main() {
    // assign a function to a name
    add := func(a, b int) int {
        return a + b
    }
    // use the name to call the function
    fmt.Println(add(3, 4))
}

// Closures, lexically scoped: Functions can access values that were
// in scope when defining the function
func scope() func() int{
    outer_var := 2
    foo := func() int { return outer_var}
    return foo
}

func another_scope() func() int{
    // won't compile because outer_var and foo not defined in this scope
    outer_var = 444
    return foo
}


// Closures: don't mutate outer vars, instead redefine them!
func outer() (func() int, int) {
    outer_var := 2
    inner := func() int {
        outer_var += 99 // attempt to mutate outer_var from outer scope
        return outer_var // => 101 (but outer_var is a newly redefined
                         //         variable visible only inside inner)
    }
    return inner, outer_var // => 101, 2 (outer_var is still 2, not mutated by foo!)
}
```

##Snippets

### HTTP Server

```go
package main

import (
    "fmt"
    "net/http"
)

// define a type for the response
type Hello struct{}

// let that type implement the ServeHTTP method (defined in interface http.Handler)
func (h Hello) ServeHTTP(w http.ResponseWriter, r *http.Request) {
    fmt.Fprint(w, "Hello!")
}

func main() {
    var h Hello
    http.ListenAndServe("localhost:4000", h)
}

// Here's the method signature of http.ServeHTTP:
// type Handler interface {
//     ServeHTTP(w http.ResponseWriter, r *http.Request)
// }
```

References:

* [https://github.com/a8m/go-lang-cheat-sheet](https://github.com/a8m/go-lang-cheat-sheet)
* [http://golang.org/](http://golang.org/)