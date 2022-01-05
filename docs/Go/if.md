# 1.6 If with a Twist

### If
- [Go Document - If](https://go.dev/tour/flowcontrol/5)
    - Go's if statements are like its for loops; the expression need not be surrounded by parentheses ( ) but the braces { } are required.
    
```go
package main

import "fmt"

func canIDrink(age int) bool {
    if age < 18 {
        return false
    }
    return true
}

func canIDrinkInKorea(age int) bool {
    if koreanAge := age + 2; koreanAge < 18 {
        return false
    }
    return true
}

func main() {
    fmt.Println(canIDrink(16))
    fmt.Println(canIDrinkInKorea(18))   
}
```

### Variable expression in if
- you can create variable before you create checking condition

- if statement can start with a short statement to execute before the condition.

- Variables declared by the statement are only in scope until the end of the if.
