# 1 Basic Types

### Common types

| | |
| :------------- | :------------- |
| <code>bool</code> |	true or false |
| <code>string</code> |	an array of characters|

### Numeric Types

| | |
| :------------- | :------------- |
| uint | either 32 or 64 bits. |
| int	| same size as uint. |
| uintptr |	an unsigned integer large enough to store the uninterpreted bits of a pointer value |
| uint8 |	the set of all unsigned 8-bit integers (0 to 255) |
| uint16 |	the set of all unsigned 16-bit integers (0 to 65535) |
| uint32 |	the set of all unsigned 32-bit integers (0 to 4294967295) |
| uint64 |	the set of all unsigned 64-bit integers (0 to 18446744073709551615) |
| int8 | the set of all signed 8-bit integers (-128 to 127) |
| int16 |	the set of all signed 16-bit integers (-32768 to 32767) |
| int32 |	the set of all signed 32-bit integers (-2147483648 to 2147483647) |
| int64	| the set of all signed 64-bit integers (-9223372036854775808 to 9223372036854775807) |
| float32 |	the set of all IEEE-754 32-bit floating-point numbers |
| float64	| the set of all IEEE-754 64-bit floating-point numbers |
| complex64 |	the set of all complex numbers with float32 real and imaginary parts |
| complex128 |	the set of all complex numbers with float64 real and imaginary parts |
| byte |	alias for uint8 |
| rune |	alias for int32 (represents a Unicode code point) |


#### Example

```go
package main

import (
	"fmt"
	"math/cmplx"
)

var (
	goIsFun bool       = true //declaring a variable of type bool
	maxInt  uint64     = 1<<64 - 1 //declaring a variable of type uint64
	complex complex128 = cmplx.Sqrt(-5 + 12i) //declaring a variable of type complex128
)

func main() {
	const f = "%T(%v)\n"
	fmt.Printf(f, goIsFun, goIsFun)
	fmt.Printf(f, maxInt, maxInt)
	fmt.Printf(f, complex, complex)
}

/*
bool(true)
uint64(18446744073709551615)
complex128((2+3i))
*/
```

# 2 Type Conversion

Converting values from one type to another is fairly simple in Go. The expression <code>T(v)</code> converts the value <code>v</code> to the type <code>T</code>.

numeric conversion:

```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
```

```go
i := 42
f := float64(i)
u := uint(f)
```

# 3. Type Assertion

A type assertion takes a value and tries to create another version in the specified explicit type.

```go
package main

import (
	"fmt"
	"time"
)

func timeMap(y interface{}) {
	z, ok := y.(map[string]interface{}) //asserting y as a map of interfaces
	if ok {
		z["updated_at"] = time.Now() //z now has the type map[string]interface
	}
}

func main() {
	foo := map[string]interface{}{
		"Matt": 42,
	}
	timeMap(foo)
	fmt.Println(foo)
}

// map[Matt:42 updated_at:2022-06-02 13:24:01.501902526 +0000 UTC]
```

The type assertion doesn’t have to be done on an empty interface. It’s often used when you have a function taking a param of a specific interface but the function inner code behaves differently based on the actual object type.

```go
package main

import "fmt"

type Stringer interface {
	String() string
}

type fakeString struct {
	content string
}

// function used to implement the Stringe interface
func (s *fakeString) String() string {
	return s.content
}

func printString(value interface{}) {
	switch str := value.(type) {
	case string:
		fmt.Println(str)
	case Stringer:
		fmt.Println(str.String())
	}
}

func main() {
	s := &fakeString{"Ceci n'est pas un string"}
	printString(s)
	printString("Hello, Gophers")

}

/*
Ceci n'est pas un string
Hello, Gophers
*/
```

* [Read more in the Effective Go guide](https://go.dev/doc/effective_go#interface_conversions)

# 4. Structs

```go
package main

import (
	"fmt"
	"time"
)

type Bootcamp struct {
	// Latitude of the event
	Lat float64
	// Longitude of the event
	Lon float64
	// Date of the event
	Date time.Time
}

func main() {
	fmt.Println(Bootcamp{
		Lat:  34.012836,
		Lon:  -118.495338,
		Date: time.Now(),
	})
}

// {34.012836 -118.495338 2022-06-02 13:28:21.714918827 +0000 UTC}
```

Declaration of struct literals:

```go
package main

import "fmt"

type Point struct {
	X, Y int
}

var (
	p = Point{1, 2}  // has type Point
	q = &Point{1, 2} // has type *Point
	r = Point{X: 1}  // Y:0 is implicit
	s = Point{}      // X:0 and Y:0
)

func main() {
	fmt.Println(p, q, r, s)
}

// {1 2} &{1 2} {1 0} {0 0}
```

Accessing fields using the dot notation:

```go
package main

import (
	"fmt"
	"time"
)

type Bootcamp struct {
	Lat, Lon float64
	Date     time.Time
}

func main() {
	event := Bootcamp{
		Lat: 34.012836,
		Lon: -118.495338,
	}
	event.Date = time.Now()
	fmt.Printf("Event on %s, location (%f, %f)",
		event.Date, event.Lat, event.Lon)
}

// Event on 2022-06-02 13:29:12.007380853 +0000 UTC, location (34.012836, -118.495338)
```

# 5. Initializing

### Using the <code>new</code> expression

```go
x := new(int)
```

```go
package main

import (
	"fmt"
)

type Bootcamp struct {
	Lat float64
	Lon float64
}

func main() {
	x := new(Bootcamp)
	y := &Bootcamp{}
	fmt.Println(*x == *y)
}

// true
```

#### Resources

* [Allocation with <code>new</code> - effective Go](https://go.dev/doc/effective_go#allocation_new)
* [Composite Literals - effective Go](https://go.dev/doc/effective_go#composite_literals)
* [Allocation with <code>make</code> - effective Go](https://go.dev/doc/effective_go#allocation_make)

# 6. Composition vs Inheritance

## Compostion

Composition (or embedding) is a well understood concept for most OOP programmers and Go supports it, here is an example of the problem it’s solving:

```go
package main

import "fmt"

type User struct {
	Id       int
	Name     string
	Location string
}

//type Player with one additional attribute

type Player struct {
	Id       int  
	Name     string
	Location string
	GameId	 int
}

func main() {
	p := Player{}
	p.Id = 42
	p.Name = "Matt"
	p.Location = "LA"
	p.GameId = 90404
	fmt.Printf("%+v", p) // the value in a default format when printing structs,
                        // the plus flag (%+v) adds field names
}

// {Id:42 Name:Matt Location:LA GameId:90404}
```

Using the dot notation to set the fields:

```go
package main

import "fmt"

type User struct {
	Id             int
	Name, Location string
}

type Player struct {
	User
	GameId int
}

func main() {
	p := Player{} //initializing
	p.Id = 42
	p.Name = "Matt"
	p.Location = "LA"
	p.GameId = 90404
	fmt.Printf("%+v", p)
}

// {User:{Id:42 Name:Matt Location:LA} GameId:90404}
```

The other option is to use a struct literal:

```go
package main

import "fmt"

type User struct {
	Id             int
	Name, Location string
}

type Player struct {
	User
	GameId int
}

func main() {
	p := Player{
		User{Id: 42, Name: "Matt", Location: "LA"},
		90404,
	}
	fmt.Printf(
		"Id: %d, Name: %s, Location: %s, Game id: %d\n",
		p.Id, p.Name, p.Location, p.GameId)
	// Directly set a field defined on the Player struct
	p.Id = 11
	fmt.Printf("%+v", p)
}

/*
Id: 42, Name: Matt, Location: LA, Game id: 90404
{User:{Id:11 Name:Matt Location:LA} GameId:90404}
*/
```

```go
package main

import "fmt"

type User struct {
	Id             int
	Name, Location string
}

func (u *User) Greetings() string {
	return fmt.Sprintf("Hi %s from %s",
		u.Name, u.Location)
}

type Player struct {
	User
	GameId int
}

func main() {
	p := Player{}
	p.Id = 42
	p.Name = "Matt"
	p.Location = "LA"
	fmt.Println(p.Greetings())
}

// Hi Matt from LA
```

Here is another example, this time we will look at implementing a <code>Job</code> struct that can also behave as a [logger](https://pkg.go.dev/log#Logger).

Here is the explicit way:

```go
package main

import (
	"log"
	"os"
)

type Job struct {
	Command string
	Logger  *log.Logger
}

func main() {
	job := &Job{"demo", log.New(os.Stdout, "Job: ", log.Ldate)}
	// same as
	// job := &Job{Command: "demo",
	//            Logger: log.New(os.Stderr, "Job: ", log.Ldate)}
	job.Logger.Print("test")
}

// Job: 2022/06/02 test
```

# 7. Exercise on Composition

## Question

<p>Looking at the <code>User</code> / <code>Player</code> example, you might have noticed that we composed <code>Player</code> using <code>User</code>. This means that a Player should be able to access methods defined in the <code>User</code> struct. In the code given below, add additional code to the <code>GreetingsForPlayer</code> function so that it uses the <code>Greetings</code> function from the <code>User</code> struct to print the string that the <code>Greetings</code> function is printing right now:</p>

### Solution:

```go
package main
import "fmt"
import "encoding/json"


type User struct {
	Id             int
	Name, Location string
}

func (u User) Greetings() string {
	return fmt.Sprintf("Hi %s from %s",
		u.Name, u.Location)
}

type Player struct {
	u User
	GameId int
}

func GreetingsForPlayer(p Player) string{
  //insert code
  return p.u.Greetings()
}

// Hi Matt from LA
```
