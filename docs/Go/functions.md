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

## Function continued
- When two or more consecutive named function parameters share a type, you can omit the type from all but the last.  

- In this example, we shortened

```go
x int, y int
```

- to

```go
x, y int
```

## Multiple results
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

## Ignore value with underscore "_"

```go
totalLength, _ := lenAndUpper("mogi") // _ underscore will ignore value 
fmt.Println(totalLength)
```

## Standard Library
- [ToUpper](https://pkg.go.dev/strings#ToUpper)

## Unlimited Arguments
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
> [stacking defer](#staking-defer)  
> [Function Values](#Function-Values)  
> [Closure](#Closure)  

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

## naked return function
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

## defer

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


## stacking defer

- Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order.

```go
package main

import "fmt"

func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}

```

```console
counting
done
9
8
7
6
5
4
3
2
1
0
```

## Function Values
- Functions are values too. They can be passed around just like other values.

- Function values may be used as function arguments and return values.

```go
import (
    "fmt"
    "math"
)

func compute(fn func(float64, float64) float64) float64 {
    // "func(float64, float64) float64" is input function type
    // "float64" is return type 
    return fn(3, 4)
}

func main() {
    //① Assign function in a variable, use a variable like function
    hypot := func(x, y float64) float64 {
        return math.Sqrt(x*x + y*y)
    }
    fmt.Println("① Call Function by a variable", hypot(5, 12))
    
    //② Assign function in compute func
    fmt.Println("② Function values as a function arguments")
    fmt.Println("compute(hypot):\t\t", compute(hypot))
    fmt.Println("compute(math.Pow):\t", compute(math.Pow))
}
```

```console
① Call Function by a variable 13
② Function values as a function arguments
compute(hypot):		 5
compute(math.Pow):	 81
```


```go
func test(x int) {
    fmt.Println("hello!", x)
}

func main() {
    x := test
    x(5)
}
```

```console
hello! 5
```

- **Call the function**
    - `x()` 
- **Reference the function**
    - if you type `x` function without `()`, `x` indicates pointer to the function

```go
func main() {
    test := func(x int) int {
        return x * -1
    }(8)
    fmt.Println(test)
}
```

```console
-8
```

```go
func test2(myFunc func(int) int) {
    // input argument type is func(int) int
    // return type is void
    fmt.Println(myFunc(7))
}

func main() {
    test := func(x int) int {
        return x * -1
    }
    test2(test) // test2 -> call test(7)
}
```

```console
-7
```


# Closure
- [Go Tour - Function Closure](https://go.dev/tour/moretypes/25)
    - *(나의 이해) ; 함수별로 선언되는 전역변수같은 느낌*
- [Go Closure Example - Conept of Go Closure](https://gobyexample.com/closures)
- Go functions may be closures. 
- A closure is a function value that references variables from outside its body. The function may access and assign to the referenced variables; in this sense the function is "bound" to the variables.


- Go 언어에서 함수는 Closure로서 사용될 수도 있다. Closure는 함수 바깥에 있는 변수를 참조하는 함수값(function value)를 일컫는데, 이때의 함수는 바깥의 변수를 마치 함수 안으로 끌어들인 듯이 그 변수를 읽거나 쓸 수 있게 된다.

- 아래 예제에서 nextValue() 함수는 int를 리턴하는 익명함수(func() int)를 리턴하는 함수이다. Go 언어에서 함수는 일급함수로서 다른 함수로부터 리턴되는 리턴값으로 사용될 수 있다. 그런데 여기서 이 익명함수가 그 함수 바깥에 있는 변수 i 를 참조하고 있다. 익명함수 자체가 로컬 변수로 i 를 갖는 것이 아니기 때문에 (만약 그렇게 되면 함수 호출시 i는 항상 0으로 설정된다) 외부 변수 i 가 상태를 계속 유지하는 즉 값을 계속 하나씩 증가시키는 기능을 하게 된다.

- 예제에서 next := nextValue() 에서 Closure 함수를 next라는 변수에 할당한 후에, 계속 next()를 3번 호출하는데 이때마다 Clouse 함수내의 변수 i는 계속 증가된 값을 가지고 있게 된다. 이것은 마치 next 라는 함수값이 변수 i 를 내부에 유지하고 있는 모양새이다. 그러나 만약 anotherNext := nextValue()와 같이 새로운 Closure 함수값을 생성한다면, 변수 i는 초기 0을 갖게 되므로 다시 1부터 카운팅을 하게 된다.

```go
package main
 
func nextValue() func() int { // "func() int" is the return type of nextValue() 
    i := 0
    return func() int {
        i++
        return i
    }
}
 
func main() {
    next := nextValue()
 
    println(next())  // 1
    println(next())  // 2
    println(next())  // 3
 
    anotherNext := nextValue()
    println(anotherNext()) // 1 다시 시작
    println(anotherNext()) // 2
}
```

- Go language provides a special feature known as an anonymous function.
- A closure is a special type of anonymous function that references variables declared outside of the function itself.

```go
package main

import "fmt"

func adder() func(int) int { // func(int) int 가 adder() 함수의 return type
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(
			pos(i),
			neg(-2*i),
		)
	}
}
```

```
0 0
1 -2
3 -6
6 -12
10 -20
15 -30
21 -42
28 -56
36 -72
45 -90
```

- Each closure has its own `sum` variable.
- So `pos` and `neg` did not share the `sum` variable.