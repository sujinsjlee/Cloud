# 1.10 Maps

> **map [key] value** 


```go
package main
import "fmt"

func main() {
	sujin := map[string]string{"name": "sujin", "age": "27"}
	for key, value := range sujin {
		fmt.Println(key, value)
	}
}
```