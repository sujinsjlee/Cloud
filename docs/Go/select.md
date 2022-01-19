# Select

```go
import (
    "fmt"
    "time"
)

func main() {
    tick := time.Tick(100 * time.Millisecond)
    boom := time.After(500 * time.Millisecond)
    for {
        select {
        case <-tick:
            fmt.Println("Passed 100\t milliseconds.")
        case <-boom:
            fmt.Println("Passed 500\t milliseconds. End!")
            return
        default:
            fmt.Println("Passed 50\t milliseconds.")
            time.Sleep(50 * time.Millisecond)
        }
    }
}
```

```console
Passed 50	 milliseconds.
Passed 50	 milliseconds.
Passed 100	 milliseconds.
Passed 50	 milliseconds.
Passed 50	 milliseconds.
Passed 100	 milliseconds.
Passed 50	 milliseconds.
Passed 50	 milliseconds.
Passed 100	 milliseconds.
Passed 50	 milliseconds.
Passed 50	 milliseconds.
Passed 100	 milliseconds.
Passed 50	 milliseconds.
Passed 50	 milliseconds.
Passed 500	 milliseconds. End!
```

- A `select` blocks until one of its cases can run, then it executes that case. It chooses one at random if multiple are ready.

- When a case is ready, the case is executed. If multiple cases are prepared, one of the prepared cases is randomly selected and executed. - When there is no prepared case, the default case in select is executed, and it is used to send or receive a value without a block.