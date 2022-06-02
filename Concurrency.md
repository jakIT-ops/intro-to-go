# 1. Concurrent Programming

### Introduction

Concurrent programming is a large topic, ​but it’s also one of the most interesting aspects of the Go language.

Concurrent programming in many environments is made difficult by the subtleties required to implement correct access to shared variables. Go encourages a different approach in which shared values are passed around on channels and, in fact, never actively shared by separate threads of execution. Only one goroutine has access to the value at any given time.

Although Go’s approach to concurrency originates in [Hoare’s Communicating Sequential Processes (CSP)](https://en.wikipedia.org/wiki/Communicating_sequential_processes), it can also be seen as a type-safe generalization of Unix pipes.

* [Rob Pike’s concurrency slides (IO 2012)](https://talks.golang.org/2012/concurrency.slide#1)
* [Video of Rob Pike at IO 2012](https://www.youtube.com/watch?v=f6kdp27TYZs)
* [Video of Concurrency is not parallelism (Rob Pike)](https://vimeo.com/49718712)

# 2. Goroutines

A goroutine is a lightweight thread managed by the Go runtime. Goroutines can be functions or methods that run concurrently with other functions or methods

```go
go f(x, y, z)
```
starts a new goroutine running:
```go
f(x, y, z)
```

Goroutines run in the same address space, so access to shared memory must be <b>synchronized</b>​​. The [sync](https://pkg.go.dev/sync) package provides useful primitives, although you won’t need them much in Go as there are other primitives.

```go
package main

import (
	"fmt"
	"time"
)

func say(s string) {
	for i := 0; i < 5; i++ {
		time.Sleep(100 * time.Millisecond)
		fmt.Println(s)
	}
}

func main() {
  go say("world") //parallel execution: both calls to say() will execute concurrently
	say("hello")
}
/*
world
hello
hello
world
hello
world
world
hello
hello
world
*/
```

# 3. Channels

Channels are pipes through which you can send and receive values using the channel operator, <code><-</code>.

```go
ch <- v    // Send v to channel ch.
v := <-ch  // Receive from ch, and
           // assign value to v.
```

```go
package main

import "fmt"

func sum(a []int, c chan int) {
	sum := 0
	for _, v := range a {
		sum += v
	}
	c <- sum // send sum to c
}

func main() {
	a := []int{7, 2, 8, -9, 4, 0}

	c := make(chan int)
	go sum(a[:len(a)/2], c)
	go sum(a[len(a)/2:], c)
	x, y := <-c, <-c // receive from c

	fmt.Println(x, y, x+y)
}

// -5 17 12
```

### Buffered channels

Channels can be buffered. Provide the buffer length as the second argument to <code>make</code> to initialize a buffered channel:

Channels can be buffered. Provide the buffer length as the second argument to make to initialize a buffered channel:

```go
package main

import "fmt"

func main() {
    c := make(chan int, 2)
    c <- 1
    c <- 2
    fmt.Println(<-c)
    fmt.Println(<-c)
}

// 1
// 2
```
But if you do:
```go
package main

import "fmt"

func main() {
	c := make(chan int, 2)
	c <- 1
	c <- 2
	c <- 3
	fmt.Println(<-c)
	fmt.Println(<-c)
	fmt.Println(<-c)
}
/*
fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan send]:
main.main()
	/usercode/main.go:9 +0xc4
exit status 2
*/
```

You are getting a deadlock!

That’s because we overfilled the buffer without letting the code a chance to read/remove a value from the channel.

However, this version using a goroutine would work fine:

```go
package main

import "fmt"

func main() {
	c := make(chan int, 2)
	c <- 1
	c <- 2
	c3 := func() { c <- 3 }
	go c3()
	fmt.Println(<-c)
	fmt.Println(<-c)
    fmt.Println(<-c)
}
/*
1
2
3
*/
```
