# 1. Introduction

> Note: A frequently asked question is “what is the difference between a function and a method”. A method is a function that has a defined receiver, in OOP terms, a method is a function on an instance of an object.

```go
package main

import (
	"fmt"
)

type User struct {
	FirstName, LastName string
}

func (u User) Greeting() string {
	return fmt.Sprintf("Dear %s %s", u.FirstName, u.LastName)
}

func main() {
	u := User{"Matt", "Aimonetti"}
	fmt.Println(u.Greeting())
}

// Dear Matt Aimonetti
```

# 2. Code Organization

```go
package main

// list of packages to import
import (
	"fmt"
)

// list of constants
const (
	ConstExample = "const before vars"
)

// list of variables
var (
	ExportedVar    = 42
	nonExportedVar = "so say we all"
)

// Main type(s) for the file,
// try to keep the lowest amount of structs per file when possible.
type User struct {
	FirstName, LastName string
	Location            *UserLocation
}

type UserLocation struct {
	City    string
	Country string
}

// List of functions
func NewUser(firstName, lastName string) *User {
	return &User{FirstName: firstName,
		LastName: lastName,
		Location: &UserLocation{
			City:    "Santa Monica",
			Country: "USA",
		},
	}
}

// List of methods
func (u *User) Greeting() string {
	return fmt.Sprintf("Dear %s %s", u.FirstName, u.LastName)
}

func main() {
  us:=User {FirstName: "Matt",
		LastName: "Damon",
		Location: &UserLocation{
			City:    "Santa Monica",
			Country: "USA",}}
  fmt.Println(us.Greeting())
}

// Dear Matt Damon
```

# 3. Type Aliasing

To define methods on a type you don’t “own”, you need to define an alias​ for the type you want to extend.


```go
package main

import (
	"fmt"
	"strings"
)

type MyStr string //using MyStr as an alias for type string

func (s MyStr) Uppercase() string {
	return strings.ToUpper(string(s))
}

func main() {
	fmt.Println(MyStr("test").Uppercase())
}

// TEST
```

```go
package main

import (
    "fmt"
    "math"
)

type MyFloat float64   //using MyFloat as an alias for type float64

func (f MyFloat) Abs() float64 {
    if f < 0 {
        return float64(-f)
    }
    return float64(f)
}

func main() {
    f := MyFloat(-math.Sqrt2)
    fmt.Println(f.Abs())
}

// 1.4142135623730951
```

# 4. Method Receivers

```go
package main

import (
	"fmt"
)

type User struct {
	FirstName, LastName string
}

func (u *User) Greeting() string { //pointers
	return fmt.Sprintf("Dear %s %s", u.FirstName, u.LastName)
}

func main() {
	u := &User{"Matt", "Aimonetti"}
	fmt.Println(u.Greeting())
}

// Dear Matt Aimonetti
```

```go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f //v will be modified directly here
	v.Y = v.Y * f
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := &Vertex{3, 4}
	v.Scale(5)
	fmt.Println(v, v.Abs())
}

// &{15 20} 25
```
