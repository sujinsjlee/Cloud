# Function1
> [Function continued](#Function-continued)  
> [Multiple results](#Multiple-results)  
> [Ignore value with underscore "_"](#Ignore-value-with-underscore-"_")  
> [Standard Library](#Standard-Library)  
> [Unlimited Arguments](#Unlimited-Arguments)  

```go
package main

import (
	"fmt"
	"strings"
)

func multiply(a, b int) int {
    // both argument is int type
    // return type is int
    return a * b
}
/*
func lenAndUpper
    @return length of the string and UPPER CASE of the string
*/
func lenAndUpper(name string) (int, string) { 
    // argument "name" is string type
    // (int, string) is return type
	return len(name), strings.ToUpper(name)
}

func repeatMe(words ...string) {
    // repeatMe function is void type (return type is not needed)
    // ...string ->  Recieve as many as argument you want
	fmt.Println(words)
}

func main() {
    
    fmt.Println(multiply(2,2))
	
    totalLen, upperName := lenAndUpper("mogi") 
	fmt.Println(totalLen, upperName)

    // totalLen := lenAndUpper("mogi")
	// fmt.Println(totalLen) -> compiler cannnot initialize 1 variable with 2 values

	totalLength, _ := lenAndUpper("mogi") // _ underscore will ignore value 
	fmt.Println(totalLength)

    repeatMe("Mon", "Tue", "Wed", "Thu", "Fri")
}
```

### Function continued
- When two or more consecutive named function parameters share a type, you can omit the type from all but the last.  

- In this example, we shortened

```go
x int, y int
```

- to

```go
x, y int
```

### Multiple results
- A function can return any number of results.  


```go
package main

import "fmt"

func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b := swap("hello", "world")
	fmt.Println(a, b)
}
```

### Ignore value with underscore "_"

```go
totalLength, _ := lenAndUpper("mogi") // _ underscore will ignore value 
fmt.Println(totalLength)
```

### Standard Library
- [ToUpper](https://pkg.go.dev/strings#ToUpper)

### Unlimited Arguments
- Recieve as many as argument you want by "variableName ...variableType"

```Go
func repeatMe(words ...string) {
    // repeatMe function is void type (return type is not needed)
    // ...string ->  Recieve as many as argument you want
	fmt.Println(words)
}
```

# Function2
> [naked return function](#naked-return-function)  
> [defer](#defer)  


```go
package main
import (
	"fmt"
	"strings"
)

func lenAndUpper(name string) (length int, uppercase string) {
	defer fmt.Println("I'm done")
	length = len(name) // length := len(name) (x) -> length is already declared on the top of the function
	uppercase = strings.ToUpper(name)
    // return length, uppercase
	return 
}

func main() {
	totalLength, up := lenAndUpper("nico")
	fmt.Println(totalLength, up)
}
```

### naked return function
- [Go Document - Naked Return Value](https://go.dev/tour/basics/7#:~:text=A%20return%20statement%20without%20arguments,harm%20readability%20in%20longer%20functions.)
- **A return statement without arguments** returns the named return values. This is known as a "naked" return.

- **Naked return statements** should be used only in short functions, as with the example shown here. **They can harm readability in longer functions.**

```go
package main

import "fmt"

func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

func main() {
	fmt.Println(split(17))
}

```

### defer

- you can do sth, after the function is finished!
- [Go Document - Defer](https://go.dev/tour/flowcontrol/12)

```go
package main

import "fmt"

func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
```


```console
> go run main.go
hello
world
```

- A defer statement defers the execution of a function until the surrounding function returns.

- The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

- 예를들어, 시스템 디자인할 때 이미지오픈하고 파일생성하고, **defer**문으로 function이 끝났을 때, 이미지를 닫던가, 파일을 닫거나 혹은 삭제하거나 API요청을하거나..등의 행동을 할 수 있음
