# 1.1 Packages and Imports

### fmt

```go
package main

import (
	"fmt"
)

func main() {
	fmt.Println("Hello, World!")
}
```

- **fmt**
    - package for formatting
    - fmt.**P**~~~
    - fmt.**F**~~~
        - fmt. (+) Upper case letter
        - if you want to export function --> you import it by **Upper case**
        - Exported from other packages --> Written in Upper case letter

### Names
- [Effective go/names](https://go.dev/doc/effective_go#names)
    - Names are as important in Go as in any other language. They even have semantic effect: the visibility of a name outside a package is determined by whether its first character is **upper case**.



```go
package main

var internalValue int
var ExternalValue int

func internalFunc() int {
  
}

func ExternalFunc() int {
  
}
```

- internalValue 와 internalFunc는 해당 패키지 내부에서만 호출이 가능
- ExternalValue 와 ExternalFunc는 패키지 외부에서도 호출이 가능
- 다른 언어의 public private개념을 대소문자를 가지고 구분