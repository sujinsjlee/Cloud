# 1.0 Main Package

### main.go

```go
package main

import (
	"fmt"
)

func main() {
	fmt.Println("Hello, World!")
}
```
- `main.go` as a file is not required. Your file name can be called whatever you want.
- However to compile, a **main package** with a func **main()** is required for executables.
    - compiler will look for a main package and then, main function as a entry point

- Running `go run main.go` will work as expected.

```console
> go run main.go
```