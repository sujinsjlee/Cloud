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
