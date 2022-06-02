<p>As Moore’s Law is reaching the end of its lifecycle, the need for concurrency is rising and consequently, the need for a programming language that enables effective implementation of concurrent programs is rising too. For this reason, Go has become one of the most popular languages in recent times. This course aims to provide a detailed introduction to Go for beginners. Furthermore, it also articulates how Go is different from traditional programming languages that programmers are accustomed to in order to acquaint programmers getting ready to delve into Go with the language too.</p>

# 1. Basics

### Variable Declaration

```go
var (
	name     string
	age      int
	location string
)
```

Or even

```go
var (
	name, location  string
	age             int
)
```

```go
var name     string
var age      int
var location string
```

### Variable Initialization

```go
var (
	name     string = "Prince Oberyn"
	age      int    =  32
	location string = "Dorne"
)
```

Multiple initialize

```go
var (
	name, location, age = "Prince Oberyn", "Dorne", 32
)
```

Inside a function, the <code>:=</code> short assignment statement can be used in place of a <code>var</code> declaration with implicit type.

```go
package main
import "fmt"

func main() {
	name, location := "Prince Oberyn", "Dorne"
	age := 32
	fmt.Printf("%s age %d from %s ", name, age, location)
}
```

Output:

<code>Prince Oberyn age 32 from Dorne </code>

A variable can contain any type, including functions:

```go
func main() {
	action := func() { //action is a variable that contains a function
		//doing something
	}
	action()
}
```

## 1.1 Constants

### Declaration

<p>Constants can only be a <b>character, string, boolean</b>, or <b>numeric</b> values and cannot be declared using the <code>:=</code> syntax. An untyped constant takes the type needed by its context.</p>

```go
const Pi = 3.14
const (
        StatusOK                   = 200
        StatusCreated              = 201
        StatusAccepted             = 202
        StatusNonAuthoritativeInfo = 203
        StatusNoContent            = 204
        StatusResetContent         = 205
        StatusPartialContent       = 206
)
```

### Example

```go
package main
import "fmt"

const (
	Pi    = 3.14
	Truth = false
	Big   = 1 << 62 // 2^62
	Small = Big >> 61
)

func main() {
	const Greeting = "ハローワールド" //declaring a constant
	fmt.Println(Greeting)
	fmt.Println(Pi)
	fmt.Println(Truth)
	fmt.Println(Big)
}
```
#### Output:
```
ハローワールド
3.14
false
4611686018427387904
```

> <b>Note:</b> The left-shift operator <code>(<<)</code> shifts its first operand left by the number of <b>bits</b> specified by its second operand. The type of the second operand <b>must</b> be an <code>int</code> or a type that has a predefined implicit numeric conversion to <code>int</code>. The right-shift operator <code>(>>)</code> shifts its first operand right by the number of <code>bits</code> specified by its second operand.

## 1.2 Printing

While you can print the value of a variable or constant using the built-in <code>print</code> and <code>println</code> functions, the more idiomatic and flexible way is to use the <code>fmt</code> [package](https://pkg.go.dev/fmt)

```go
package main
import "fmt"

func main() {
	cylonModel := 6
	fmt.Println(cylonModel)
}
```
```go
package main
import "fmt"

func main() {
	name := "Caprica-Six"
	aka := fmt.Sprintf("Number %d", 6)
	fmt.Printf("%s is also known as %s",
		name, aka) //printing variables within the string
}

// Caprica-Six is also known as Number 6
```

## 1.3 Package and Imports

Every Go program is made up of packages. Programs start running in package main.

```go
package main
import "fmt"

func main() {
	fmt.Printf("Hello, World!\n")
}
```

#### Import statement examples

```go
import "fmt"
import "math/rand"
```
Or grouped:
```go
import (
    "fmt"
    "math/rand"
)
```

* [Go Tour: Packages](https://go.dev/tour/basics/2)
* [Go Tour: Imports](https://go.dev/tour/basics/1)

```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	fmt.Println("My favorite number is", rand.Intn(10))
}

// My favorite number is 1
```

```go
package main

import (
	"fmt"
	"math"
)

func main() {
	fmt.Printf("Now you have %g problems.", math.Sqrt(7))
}

// Now you have 2.6457513110645907 problems.
```

To import the crypto package, you would need to use the following import statement:

```go
import "github.com/mattetti/goRailsYourself/crypto"
```

## 1.4 Code Location

### Getting code location



```go
$ go get github.com/mattetti/goRailsYourself/crypto
```

This command will pull down the code and put it in your Go path. When installing Go, we set the <code>GOPATH</code> environment variable and that is what’s used to store binaries and libraries. That’s also where you should store your code (your workspace).

```go
$ ls $GOPATH
bin	pkg	src
```

```go
$ ls $GOPATH/src
bitbucket.org	code.google.com	github.com	launchpad.net
```

## 1.5 Exported names

Use the provided Go [documentation](https://pkg.go.dev/std) or [godoc.org](https://pkg.go.dev/) to find exported names.

## 1.6 Functions and Return values

The return type(s) are then specified after the function name and inputs, before writing the definition. Functions can be defined to return any number of values and each of them are put here.

```go
package main
import "fmt"

func add(x int, y int) int {
    return x + y
}

func main() {
    fmt.Println(add(42, 13))
}
```

In the following example, instead of declaring the type of each parameter, we only declare one type that applies to both.

```go
package main
import "fmt"

func add(x, y int) int {
    return x + y
}

func main() {
    fmt.Println(add(42, 13))
}
```

### Return values

```go
package main

import "fmt"

func location(city string) (string, string) {
	var region string
	var continent string

	switch city {
	case "Los Angeles", "LA", "Santa Monica":
		region, continent = "California", "North America"
	case "New York", "NYC":
		region, continent = "New York", "North America"
	default:
		region, continent = "Unknown", "Unknown"
	}
	return region, continent
}

func main() {
	region, continent := location("Santa Monica")
	fmt.Printf("Matt lives in %s, %s", region, continent)
}

// Matt lives in California, North America
```

### Named Results

```go
package main
import "fmt"

func location(city string) (region, continent string) {
	switch city {
	case "Los Angeles", "LA", "Santa Monica":
		region, continent = "California", "North America"
	case "New York", "NYC":
		region, continent = "New York", "North America"
	default:
		region, continent = "Unknown", "Unknown"
	}
	return //returning region and continent
}

func main() {
	region, continent := location("Santa Monica")
	fmt.Printf("Matt lives in %s, %s", region, continent)
}

// Matt lives in California, North America
```

## 1.7 Pointers

Go has pointers, but no pointer arithmetic. Struct fields can be accessed through a struct pointer. The indirection through the pointer is transparent (you can directly call fields and methods on a pointer).

```go
client := &http.Client{}
resp, err := client.Get("http://gobootcamp.com")
```

## 1.8 Mutability

In Go, only constants are <code>immutable</code>. However, because arguments are passed by value, a function receiving a value argument and mutating it, won’t mutate the original value.

```go
package main
import "fmt"

type Artist struct {
	Name, Genre string
	Songs       int
}

func newRelease(a Artist) int { //passing an Artist by value
	a.Songs++
	return a.Songs
}

func main() {
	me := Artist{Name: "Matt", Genre: "Electro", Songs: 42}
	fmt.Printf("%s released their %dth song\n", me.Name, newRelease(me))
	fmt.Printf("%s has a total of %d songs", me.Name, me.Songs)
}

/*
Matt released their 43th song
Matt has a total of 42 songs
*/
```

```go
package main
import "fmt"

type Artist struct {
	Name, Genre string
	Songs       int
}

func newRelease(a *Artist) int { //passing an Artist by reference
	a.Songs++
	return a.Songs
}

func main() {
	me := &Artist{Name: "Matt", Genre: "Electro", Songs: 42}
	fmt.Printf("%s released their %dth song\n", me.Name, newRelease(me))
	fmt.Printf("%s has a total of %d songs", me.Name, me.Songs)
}

/*
Matt released their 43th song
Matt has a total of 43 songs
*/
```
