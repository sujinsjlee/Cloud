# 3.0 ~ 3.2 URL Checker & GO ROUTINES

> main.go


```go
package main

import (
	"errors"
	"fmt"
	"net/http"
)

var errRequestFailed = errors.New("Request failed")

func main() {
	var results = make(map[string]string)
	urls := []string{
		"https://www.airbnb.com/",
		"https://www.google.com/",
		"https://www.amazon.com/",
		"https://www.reddit.com/",
		"https://www.google.com/",
		"https://soundcloud.com/",
		"https://www.facebook.com/",
		"https://www.instagram.com/",
		"https://academy.nomadcoders.co/",
	}
	for _, url := range urls {
		result := "OK"
		err := hitURL(url)
		if err != nil {
			result = "FAILED"
		}
		results[url] = result
	}
	for url, result := range results {
		fmt.Println(url, result)
	}
}

func hitURL(url string) error {
	fmt.Println("Checking:", url)
	resp, err := http.Get(url)
	if err != nil || resp.StatusCode >= 400 {
		fmt.Println(err, resp.StatusCode)
		return errRequestFailed
	}
	return nil
}
```

```console
> go run main.go
Checking: https://www.airbnb.com/
Checking: https://www.google.com/
Checking: https://www.amazon.com/
Checking: https://www.reddit.com/
Checking: https://www.google.com/
Checking: https://soundcloud.com/
Checking: https://www.facebook.com/
Checking: https://www.instagram.com/
Checking: https://academy.nomadcoders.co/
https://soundcloud.com/ OK
https://www.facebook.com/ OK
https://www.instagram.com/ OK
https://academy.nomadcoders.co/ OK
https://www.airbnb.com/ OK
https://www.google.com/ OK
https://www.amazon.com/ OK
https://www.reddit.com/ OK
```

-  **maps - make()**
    - [Declaration and Initialization](https://go.dev/blog/maps)
    - A Go map type looks like this:
        
        ```go
        map[KeyType]ValueType
        ```
    
    - where KeyType may be any type that is comparable (more on this later), and ValueType may be any type at all, including another map!

    - This variable m is a map of string keys to int values:

        ```go
        var m map[string]int
        ```
    
    - Map types are reference types, like pointers or slices, and so the value of m above is nil; it doesn’t point to an initialized map. A nil map behaves like an empty map when reading, but **attempts to write to a nil map will cause a runtime panic;** don’t do that. To initialize a map, use the built in make function:
        
        ```go
        m = make(map[string]int)
        ```
    
    - The **make function** allocates and initializes a hash map data structure and returns a map value that points to it. The specifics of that data structure are an implementation detail of the runtime and are not specified by the language itself. In this article we will focus on the use of maps, not their implementation.


## Goroutines
- `NOT TOP-DOWN PROGRAMMING, BUT CONCURRENCY!!`  
- **Goroutines** : A Goroutine is a function or method which executes **independently and simultaneously** in connection with any other Goroutines present in your program.
    - [Goroutines](https://go.dev/tour/concurrency/1)


> BEFORE Gorutines

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	goodCount("AAA")
	goodCount("BBB")
}

func goodCount(person string) {
	for i := 0; i < 5; i++ {
		fmt.Println(person, "is good", i)
		time.Sleep(time.Second)
	}
}
```

```console
> go run main.go
AAA is good 0
AAA is good 1
AAA is good 2
AAA is good 3
AAA is good 4
BBB is good 0
BBB is good 1
BBB is good 2
BBB is good 3
BBB is good 4
```

> **AFTER Gorutines**

- In Golang, concurrency is achieved by GoRoutines, which is just a function with the keyword "go" in front of it and it is capable of running concurrently with the other functions.


```go
package main

import (
	"fmt"
	"time"
)

func main() {
	go goodCount("AAA")
	goodCount("BBB")
}

func goodCount(person string) {
	for i := 0; i < 5; i++ {
		fmt.Println(person, "is good", i)
		time.Sleep(time.Second)
	}
}
```

```console
> go run main.go
AAA is good 0
BBB is good 0
AAA is good 1
BBB is good 1
AAA is good 2
BBB is good 2
AAA is good 3
BBB is good 3
AAA is good 4
BBB is good 4
```


> **What's happening???** - GO does not wait main() to finish  

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	go goodCount("AAA")
	go goodCount("BBB")
    fmt.Println("hello from main")
}

func goodCount(person string) {
	for i := 0; i < 5; i++ {
		fmt.Println(person, "is good", i)
		time.Sleep(time.Second)
	}
}
```

```console
> go run main.go
hello from main
```

- You may notice that the code above only prints the message "hello from main" 
- This is because **the main function itself is also a GoRoutine**, which runs concurrently with our print function. 
- In this case, the main GoRoutine will not wait for another GoRoutine to complete and it will execute the line fmt.Println("hello from main") immediately and exit before the "is good" statement can be printed its message.


> **Waiting for the GOROUTINE**

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	go goodCount("AAA")
	go goodCount("BBB")
	time.Sleep(time.Second * 5)
}

func goodCount(person string) {
	for i := 0; i < 5; i++ {
		fmt.Println(person, "is good", i)
		time.Sleep(time.Second)
	}
}
```

```console
BBB is good 0
AAA is good 0
BBB is good 1
AAA is good 1
BBB is good 2
AAA is good 2
AAA is good 3
BBB is good 3
AAA is good 4
BBB is good 4
```


- In the second example, the main GoRoutine is forced to sleep for a second before the main function exits.
-  In this example, you can treat the **time.Sleep()** function as a "blocker" which **blocks the main GoRoutine for one second.** 