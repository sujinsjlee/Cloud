# 1.2 Variables and Constants

## Constants

```go
package main

import "fmt"

func main() {
	const name string = "mogi"
	//name = "sujin" -> cannot assign to name
	fmt.Println(name)
}
```

- Constants are declared like variables, but with the const keyword.
- Constants can be character, string, boolean, or numeric values.
- Constants cannot be declared using the **:=** syntax.

## Variable

```go
package main

import "fmt"

func main() {
	var name string = "mogi"
	name = "sujin" // can assign to name
	fmt.Println(name)
}
```

## Short variable declarations := 

```go
package main

import "fmt"

// isName := false -> cannot assign value with := outside of the function
var isName bool = false

func main() {
	name := "mogi"
    // name = false -> cannot change the variable type after := declaration
	name = "sujin"
	fmt.Println(name)
}
```

- `Go` can **guess** the type for you by **:=** 
    - you won't be able to change the type when you declare variable with **:=**
- 축약형은 func 안에서만, func 밖에서는 원래 선언문을 그대로 작성
    - Inside a function, the := short assignment statement can be used in place of a var declaration with implicit type.
    - Outside a function, every statement begins with a keyword (var, func, and so on) and so the := construct is not available.