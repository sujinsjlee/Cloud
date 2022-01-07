# 1.5 for, range, args

### For
- [Go Document - For](https://go.dev/tour/flowcontrol/1) 

### The init and post statements are optional
### For is Go's "while"
- At that point you can drop the semicolons: C's while is spelled for in Go.

```go
package main

import "fmt"

func main() {
    sum := 1
    for ; sum < 1000 ; {
        sum += sum
    }
    fmt.Println(sum) //1024

    idx := 1
    for idx < 1000 {
        idx += idx
    }
    fmt.Println(idx) //1024
}
```

```go
package main

import "fmt"

func superAdd(numbers ...int) {
    for i := 0; i < len(numbers); i++ {
        fmt.Println(i) 
    }
}

func main() {
    superAdd(1, 2, 3, 4, 5, 6)
}
``` 

```console
> go run main.go
0
1
2
3
4
5
```


### Range

```go
package main

import "fmt"

func superAdd(numbers ...int) {
    for number := range numbers {
        fmt.Println(number) 
    }
}

func main() {
    superAdd(1, 2, 3, 4, 5, 6)
    // 0 1 2 3 4 5 (o)
    // 1 2 3 4 5 6 (x)
}
``` 

```console
> go run main.go
0
1
2
3
4
5
```
- `range` returns **index**
- [Go Document - Range](https://go.dev/tour/moretypes/16)
    - The range form of the for loop iterates over a slice or map.
    - When ranging over a slice, two values are returned for each iteration. 
        - The first is **the index**
        - The second is **a copy of the element at that index**
            ```go
            package main

            import "fmt"

            var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

            func main() {
                for i, v := range pow {
                    fmt.Printf("2^%d = %d\n", i, v)
                }
            }
            ```

            ```console
            2^0 = 1
            2^1 = 2
            2^2 = 4
            2^3 = 8
            2^4 = 16
            2^5 = 32
            ```

```go
package main

import "fmt"

func superAdd(numbers ...int) {
    for index, number := range numbers {
        fmt.Println(index, number) 
    }
}

func main() {
    superAdd(1, 2, 3, 4, 5, 6)
}
``` 

```console
> go run main.go
0 1
1 2
2 3
3 4
4 5
5 6
```


```go
package main

import "fmt"

func superAdd(numbers ...int) int {
    total := 0
    for _, number := range numbers {
        total += number
    }
    return total
}

func main() {
    result := superAdd(1, 2, 3, 4, 5, 6)
    fmt.Println(result)
}
```

### Range continued
- You can **skip** the index or value by assigning to _.

- for i, _ := range pow
- for _, value := range pow
- If you only want the index, you can omit the second variable.
    - for i := range pow

