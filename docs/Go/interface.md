# Interfaces

- An interface type is defined as a set of method signatures.
- [Go Interface](http://golang.site/go/article/18-Go-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4)

## Go Interface
- If a **struct** is a collection of fields, an **interface is a collection of methods.**
-  An interface defines the method prototypes that a type must implement. In order for a user-defined type to implement an interface, simply implement all methods of the interface.

- Interfaces are defined using the type statement just like structs.

```go
type Shape interface {
    area() float64 // 면적
    perimeter() float64 // 둘레
}
```

## Implement Interface

```go
//Interface
type Shape interface {
    area() float64
    perimeter() float64
}

// Rect 
type Rect struct {
    width, height float64
}
 
// Circle 
type Circle struct {
    radius float64
}
 
// Implement Shape Interface typed Rect
func (r Rect) area() float64 { return r.width * r.height }
func (r Rect) perimeter() float64 {
     return 2 * (r.width + r.height)
}
  
// Implement Shape Ingerface typed Circle
func (c Circle) area() float64 { 
    return math.Pi * c.radius * c.radius
}
func (c Circle) perimeter() float64 { 
    return 2 * math.Pi * c.radius
}
```

- In order to implement the interface, the corresponding type only needs to implement all the methods of the interface. 
- To implement the above Shape interface, only two methods `area()` and `perimeter()` need to be implemented. 
    - For example, when there are two types, Rect and Circle, to implement the Shape interface, implement two methods for each type as shown above.

## Usage of Interface

```go
func main() {
    r := Rect{10., 20.}
    c := Circle{10}
 
    showArea(r, c)
}
 
func showArea(shapes ...Shape) { // interface as a parameter
    for _, s := range shapes {
        a := s.area() // call interface method
        println(a)
    }
}
```

- A common example of using an interface is when a function accepts an **interface as a parameter.** When a function parameter is an interface, it means that any type can be used as an input parameter as long as the interface is implemented.

- In the example above, the `showArea()` function accepts Shape interfaces as parameters, so type objects that implement Shape interfaces like Rect and Circle can be received as parameters. In the `showArea()` function, you can use the methods of the corresponding interface, that is, `area()` or `perimeter()`.

## Empty Interface
> The empty interface is usually used when dealing with values of unknown type


```go
func Marshal(v interface{}) ([]byte, error);
 
func Println(a ...interface{}) (n int, err error);
```

- When programming Go, you often come across an **empty interface**, which is often called an interface type. 
- For example, if you look at the function prototypes of several standard packages, you can see that empty interfaces appear frequently as shown above. An empty interface is expressed as `interface{}`.

```go
package main
 
import "fmt"
 
func main() {
    var x interface{}
    x = 1 
    x = "Tom"
 
    printIt(x)
}
 
func printIt(v interface{}) {
    fmt.Println(v) //Tom
}
```

- The Empty interface is an empty interface that has no methods at all. Every Type in Go implements at least 0 methods, so it is common to use an empty interface to represent all Types in Go. 
- In other words, the empty interface can be viewed as a container that can contain any type, and it can be viewed as a **Dynamic type** commonly referred to in various languages. (Note: An empty interface can be viewed as an object in C# and Java, and the same as `void*` in C/C++)

- In the example above, interface type x contains the integer 1 and then the string Tom again, and the execution result outputs the last contained Tom.

## Type Assertion

```go
func main() {
    var a interface{} = 1
 
    i := a       // a, i is dynamic type, value is 1
    j := a.(int) // j is int type, value is 1 -> Type Assertion
 
    println(i)  // print pointer address
    println(j)  // print 1 
}
```

- When expressed as `x.(T)` for x of interface type and type T, this asserts that x is not nil and that x belongs to type T. This expression is called **“Type Assertion”**. 
- If x is nil or the type of x is not T, a runtime error will occur. 
- **If x is of type T, x of type T is returned.** That is, in the example above, the variable j becomes an int type variable j from a.(int).