# 1.7 Switch

### Switch
- [Go Document - Switch](https://go.dev/tour/flowcontrol/9)

```go
package main
import "fmt"

func canIDrink(age int) bool {
    switch koreanAge := age + 2; koreanAge {
    case koreanAge < 18:
        return false
    case koreanAge >= 18:
        return true
    }
    default:
        fmt.Println("default statement")
    return false
}

func main() {
    fmt.Println(canIDrink(18))
}
```

### Switch with no condition
- Switch without a condition is the same as switch true.

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
}
```

### fallthrough - Execute next case

```go
import (
    "fmt"
)

func no_fallthrough(score int) {
    var grade string
    switch {
    case score > 90:
        grade = "A"
    case score > 70:
        grade = "B"
    case score > 50:
        grade = "C"
    default:
        grade = "F"
    }
    
    fmt.Println("Without fallthrough: ", grade)
}

func yes_fallthrough(score int) {
    var grade string
    switch {
    case score > 90:
        grade = "A"
        fallthrough
    case score > 70:
        grade = "B"
        fallthrough
    case score > 50:
        grade = "C"
        fallthrough
    default:
        grade = "F"
    }
    
    fmt.Println("With fallthrough: ", grade)
}
func main() {
    fmt.Println("What is my grade??")
    
    score := 80
    yes_fallthrough(score)
    no_fallthrough(score)
}
```

```console
What is my grade??
With fallthrough:  F 
Without fallthrough:  B 
```

- A fallthrough statement transfers control to the next case.


## Exit with break

```go
Loop:
    for _, ch := range "a b\nc" {
        switch ch {
        case ' ': // skip space
            break
        case '\n': // break at newline
            break Loop
        default:
            fmt.Printf("%c\n", ch)
        }
    }
```

```console
a
b
```

- A **break** statement terminates execution of the innermost for, switch, or select statement.

- If you need to break out of a surrounding loop, not the switch, **you can put a label on the loop** and break to that label. This example shows both uses.