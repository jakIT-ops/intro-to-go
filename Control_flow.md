# 1. IF Statement

```go
package main

import (
	"fmt"
	"math"
)

func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}

func main() {
	fmt.Println(sqrt(2), sqrt(-4))
}

// 1.4142135623730951 2i
```

```go
package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	}
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}

// 9 20
```

* [Example of an <code>if</code> and <code>else</code> block:](https://go.dev/tour/flowcontrol/7)

```go
package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
	// can't use v here, though
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}

/*
27 >= 20
9 20
*/
```

# 2. FOR Loop

### for Loop Syntax

```go
package main

import "fmt"

func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}

// 45
```

### For Loops Without Statements

```go
package main

import "fmt"

func main() {
	sum := 1
	for ; sum < 1000; { //iterate as long as sum<1000
		sum += sum
	}
	fmt.Println(sum)
}

// 1024
```

### For loop as an Alternative to While

```go
package main

import "fmt"

func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}

// 1024
```

# 3. Switch Case Statement

### Alternative to Multiple ifelse Statements#

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	now := time.Now().Unix()
	mins := now % 2
	switch mins {
	case 0:
		fmt.Println("even")
	case 1:
		fmt.Println("odd")
	}
}

// even
```

### Key Features

```go
package main

import "fmt"

func main() {
	num := 3
	v := num % 2
	switch v {
	case 0:
		fmt.Println("even")
	case 3 - 2:
		fmt.Println("odd")
	}
}

// odd
```

You can execute all the following statements after a match using the <code>fallthrough</code> statement:

```go
package main

import "fmt"

func main() {
	n := 4
	switch n {
	case 0:
		fmt.Println("is zero")
		fallthrough //if case matches, all following conditions will be executed as well
	case 1:
		fmt.Println("is <= 1")
		fallthrough
	case 2:
		fmt.Println("is <= 2")
		fallthrough
	case 3:
		fmt.Println("is <= 3")
		fallthrough
	case 4:
		fmt.Println("is <= 4")
		fallthrough
	case 5:
		fmt.Println("is <= 5")
		fallthrough
	case 6:
		fmt.Println("is <= 6")
		fallthrough
	case 7:
		fmt.Println("is <= 7")
		fallthrough
	case 8:
		fmt.Println("is <= 8")
		fallthrough
	default:
		fmt.Println("Try again!")
	}
}

/*

1.2s
is <= 4
is <= 5
is <= 6
is <= 7
is <= 8
Try again!
*/
```

# 3. Exercise on For Loops

We have learnt that <code>for</code> loops are used as <code>while</code> loops in Go. Given below is a C++ code that uses a <code>while</code> loop to sum an array of numbers. Rewrite the code in Go so that it uses a <code>for</code> loop to sum a given list of numbers:

```go
package main
import "fmt"
import "strconv"
import "encoding/json"

func GetSum(array [] int) int {
  sum:=0
    for i := 0; i < len(array); i++ {
        sum=sum+array[i];
    }
  return sum
}

// Input : 0 1 2 5
// Output : 8
```
