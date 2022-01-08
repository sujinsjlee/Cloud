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