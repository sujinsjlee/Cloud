# 1.11 Structs

```go
package main

import "fmt"

type person struct {
    name    string
    age     int
    favFood []string
}

func main() {
    favFood := []string{"kimchi", "ramen"}
    sujin := person{name: "sujin", age: 27, favFood: favFood}
    fmt.Println(sujin.name)
}
```

- go doesnt' have
    - class
    - constructor method
    - object
    - everything you need like method, constructor -> will be covered by **struct**

## Struct Declaration and usage

```go
import "fmt"

type Vertex struct {
    X int
    Y int
}

// struct instance declaration  
var (
    //① common way X is 1, Y is 2
    v1 = Vertex{1, 2}
    //② Declare only X as 1, Y is set as zero value
    v2 = Vertex{X: 1}
    //③ Both X, Y is set as zero value
    v3 = Vertex{}
)

func main() {
    fmt.Println("v1.X: ", v1.X)
    v1.X = 4
    fmt.Println("Change with v1.X = 4: ", v1.X)
    
    //④ possible to change the value by struct pointer
    var p  = &v1
    p.X = 10
    fmt.Println("Change value with pointer struct: v1.X: ", v1.X)
}
```

```console
> go run main.go
v1.X: 1
Change with v1.X = 4: 4
Change value with pointer struct: v1.X: 10
```