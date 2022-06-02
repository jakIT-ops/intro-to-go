# 1. Working with Arrays

### Arrays

The type <code>[n]T</code> is an array of <code>n</code> values of type <code>T</code>.

```go
var a [10]int
```

```go
package main

import "fmt"

func main() {
    var a [2]string   //array of size 2
    a[0] = "Hello"    //Zero index of "a" has "Hello"
    a[1] = "World"    //1st index of "a" has "World"
    fmt.Println(a[0], a[1]) // will print Hello World
    fmt.Println(a)           // will print [Hello World]
}

/*
Hello World
[Hello World]
*/
```

```go
package main

import "fmt"

func main() {
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)

	primes := [6]int{2, 3, 5, 7, 11, 13}  // array of prime numbers of size 6
	fmt.Println(primes)
}

/*
Hello World
[Hello World]
[2 3 5 7 11 13]
*/
```
You can also set the array entries as you declare the array:
```go
package main

import "fmt"

func main() {
    a  := [2]string{"hello", "world!"}
    fmt.Printf("%q", a)
}

// ["hello" "world!"]
```
Finally, you can use an <b>ellipsis</b> to use an implicit length when you pass the values:
```go
package main

import "fmt"

func main() {
	a := [...]string{"hello", "world!"}
	fmt.Printf("%q", a)
}

// ["hello" "world!"]
```

#### Printing Arrays

If we had used <code>Println</code> or the <code>%s</code> verb, we would have had a different result:

```go
package main

import "fmt"

func main() {
	a := [2]string{"hello", "world!"}
	fmt.Println(a)
	// [hello world!]
	fmt.Printf("%s\n", a)
	// [hello world!]
	fmt.Printf("%q\n", a)
	// ["hello" "world!"]
}

/*
[hello world!]
[hello world!]
["hello" "world!"]
*/
```

#### Multi-dimensional Arrays

```go
package main

import "fmt"

func main() {
	var a [2][3]string
	for i := 0; i < 2; i++ {
		for j := 0; j < 3; j++ {
			a[i][j] = fmt.Sprintf("row %d - column %d", i+1, j+1)
		}
	}
	fmt.Printf("%q", a)
	// [["row 1 - column 1" "row 1 - column 2" "row 1 - column 3"]
	//  ["row 2 - column 1" "row 2 - column 2" "row 2 - column 3"]]
}

// [["row 1 - column 1" "row 1 - column 2" "row 1 - column 3"] ["row 2 - column 1" "row 2 - column 2" "row 2 - column 3"]]
```

# 2. Slices in GO

## Slicing a slice

```go
package main

import "fmt"

func main() {
	mySlice := []int{2, 3, 5, 7, 11, 13}
	fmt.Println(mySlice)
	// [2 3 5 7 11 13]

	fmt.Println(mySlice[1:4])
	// [3 5 7]

	// missing low index implies 0
	fmt.Println(mySlice[:3])
	// [2 3 5]

	// missing high index implies len(s)
	fmt.Println(mySlice[4:])
	// [11 13]
}
```

```go
package main
import "fmt"

func main() {
	names := [4]string{
		"John",
		"Paul",
		"George",
		"Ringo",
	}
	fmt.Println(names)

	a := names[0:2]    //slice a
	b := names[1:3]      //slice b
	fmt.Println(a, b)

	b[0] = "XXX"      // value at zeroth index of slice b changed
	fmt.Println(a, b)
	fmt.Println(names)
}

/*
[John Paul George Ringo]
[John Paul] [Paul George]
[John XXX] [XXX George]
[John XXX George Ringo]
*/
```

#### Making slices

```go
package main

import "fmt"

func main() {
	cities := make([]string, 3)
	cities[0] = "Santa Monica"
	cities[1] = "Venice"
	cities[2] = "Los Angeles"
	fmt.Printf("%q", cities)
	// ["Santa Monica" "Venice" "Los Angeles"]
}
```

#### Appending to a slice

```go
package main

import "fmt"

func main() {
	cities := []string{}
	cities = append(cities, "San Diego")
	fmt.Println(cities)
	// [San Diego]
}
```

```go
package main

import "fmt"

func main() {
	cities := []string{}
	cities = append(cities, "San Diego", "Mountain View")
	fmt.Printf("%q", cities)
	// ["San Diego" "Mountain View"]
}
```

```go
package main

import "fmt"

func main() {
	cities := []string{"San Diego", "Mountain View"}
	otherCities := []string{"Santa Monica", "Venice"}
	cities = append(cities, otherCities...)
	fmt.Printf("%q", cities)
	// ["San Diego" "Mountain View" "Santa Monica" "Venice"]
}
```

#### Length

```go
package main

import "fmt"

func main() {
	cities := []string{
		"Santa Monica",
		"San Diego",
		"San Francisco",
	}
	fmt.Println(len(cities))
	// 3
	countries := make([]string, 42)
	fmt.Println(len(countries))
	// 42
}
```

#### Nil slices

The zero value of a slice is nil. A nil slice has a length and capacity of 0.

```go
package main

import "fmt"

func main() {
    var z []int
    fmt.Println(z, len(z), cap(z))
    // [] 0 0
    if z == nil {
        fmt.Println("nil!")
    }
    // nil!
}
```

### Resources

* [Go slices, usage and internals](http://golang.org/doc/articles/slices_usage_and_internals.html)
* [Effective Go - slices](http://golang.org/doc/effective_go.html#slices)
* [Append function documentation](https://pkg.go.dev/builtin#append)
* [Slice tricks](https://github.com/golang/go/wiki/SliceTricks)
* [Effective Go - slices](https://go.dev/doc/effective_go#slices)
* [Effective Go - two-dimensional slices](https://go.dev/doc/effective_go#two_dimensional_slices)
* [Go by example - slices](https://gobyexample.com/slices)

# 3. Range in for loops

### Range

```go
package main

import "fmt"

var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
    for i, v := range pow { //loops over the length of pow
        fmt.Printf("2**%d = %d\n", i, v)
    }
}

/*
2**0 = 1
2**1 = 2
2**2 = 4
2**3 = 8
2**4 = 16
2**5 = 32
2**6 = 64
2**7 = 128
*/
```

```go
package main

import "fmt"

func main() {
    pow := make([]int, 10)
    for i := range pow {
        pow[i] = 1 << uint(i)
    }
    for _, value := range pow {
        fmt.Printf("%d\n", value)
    }
}

/*
1
2
4
8
16
32
64
128
256
512
*/
```

### Break & continue

```go
package main

import "fmt"

func main() {
	pow := make([]int, 10)
	for i := range pow {
		pow[i] = 1 << uint(i)
		if pow[i] >= 16 {
			break //stop iterating over pow when it reaches 16
		}
	}
	fmt.Println(pow)
	// [1 2 4 8 16 0 0 0 0 0]
}
```

```go
package main

import "fmt"

func main() {
	pow := make([]int, 10)
	for i := range pow {
		if i%2 == 0 {
			continue //skip each even index of pow
		}
		pow[i] = 1 << uint(i)
	}
	fmt.Println(pow)
	// [0 2 0 8 0 32 0 128 0 512]
}
```

### Range and maps

```go
package main

import "fmt"

func main() {
	cities := map[string]int{
		"New York":    8336697,
		"Los Angeles": 3857799,
		"Chicago":     2714856,
	}
	for key, value := range cities { //for each key-value pair in cities
		fmt.Printf("%s has %d inhabitants\n", key, value)
	}
}

/*
Chicago has 2714856 inhabitants
New York has 8336697 inhabitants
Los Angeles has 3857799 inhabitants
*/
```

# 4. Maps in Go

```go
package main

import "fmt"

func main() {
	celebs := map[string]int{ //mapping strings to integers
		"Nicolas Cage":       50,
		"Selena Gomez":       21,
		"Jude Law":           41,
		"Scarlett Johansson": 29,
	}

	fmt.Printf("%#v", celebs)
}

// map[string]int{"Jude Law":41, "Scarlett Johansson":29, "Nicolas Cage":50, "Selena Gomez":21}
```

```go
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m map[string]Vertex

func main() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{40.68433, -74.39967} //assignment
	fmt.Println(m["Bell Labs"])
}

// {40.68433 -74.39967}
```

When using map literals, if the top-level type is just a type name, you can omit it from the elements of the literal.

```go
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	// same as "Bell Labs": Vertex{40.68433, -74.39967}
	"Google": {37.42202, -122.08408},
}

func main() {
	fmt.Println(m)
}

// map[Bell Labs:{40.68433 -74.39967} Google:{37.42202 -122.08408}]
```

### Mutating Maps

Insert or update an element in map m:
```go
m[key] = elem
```
Retrieve an element:
```go
elem = m[key]
```
Delete an element:
```go
delete(m, key)
```
Test that a key is present with a two-value assignment:
```go
elem, ok = m[key]
```
Let’s take a look at an example now:
```go
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	// same as "Bell Labs": Vertex{40.68433, -74.39967}
	"Google": {37.42202, -122.08408},
}

func main() {
  m["Splice"] = Vertex{34.05641, -118.48175} //inserting a new (key,value) here
	fmt.Println(m["Splice"])
	delete(m, "Splice")    //deleting the element
	fmt.Printf("%v\n", m)
	name, ok := m["Splice"]      //checks to see if element is present
	fmt.Printf("key 'Splice' is present?: %t - value: %v\n", ok, name)
	name, ok = m["Google"]
	fmt.Printf("key 'Google' is present?: %t - value: %v\n", ok, name)
}

/*
{34.05641 -118.48175}
map[Bell Labs:{40.68433 -74.39967} Google:{37.42202 -122.08408}]
key 'Splice' is present?: false - value: {0 0}
key 'Google' is present?: true - value: {37.42202 -122.08408}
*/
```

### Resources

* [Go team blog post on maps](https://go.dev/blog/maps)
* [Effective Go - maps](https://go.dev/doc/effective_go#maps)

# 5. Exercise in Maps

### Questions

The code below should return a map of the counts of each “word” in the string <code>s</code>.

You might find [strings.Fields](https://pkg.go.dev/strings#Fields) helpful in order to solve this question.

```go
package main

import (
	"fmt"
	"strings"
    "encoding/json"
    "strconv"
)

func WordCount(s string) map[string]int {
	words := strings.Fields(s)
	count := map[string]int{}
	for _, word := range words {
		count[word]++
	}
	return count
}

// Input: I am learning Go!
// Output: I: 1 am: 1 learning: 1Go!: 1
```
