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

## Memory Usage and Gorutines
> goroutine은 같은 주소 공간을 쓰기 때문에, shared memory에 접근할 때는 동기화해줘야 합니다.  
> **Don’t communicate by sharing memory, share memory by communicating.**

- [memory usage](https://www.huskyhoochu.com/go-concurrency/)
- 그니까 주소공간이 같다는게 변수나 함수등이 메모리에 차지하는 주소값이 같으므로 메모리 공유 sharing memory를 권장하지 않는 것으로 보임

- 메모리 0x00에 여러 개의 쓰레드가 동시에 접근해 값을 증가시킨다면 어떤 일이 벌어질까? 쓰레드 1은 쓰레드 2가 증가시킨 값을 확인하지 못한 채 원본 메모리의 값을 증가시키는 행위를 반복할 것이다. 그 사이에 쓰레드 2가 자신이 증가시킨 값을 다시 메모리에 덮어씌우면 쓰레드 1이 했던 작업은 지워져버리고 말 것이다. 그래서 우리가 원하는 만큼 값이 증가되지 않는 일이 벌어질 것이다.

- mutex를 사용하면 그 순간의 실행흐름을 “잠금”할 수 있어 다른 고루틴이 작업이 실행 중인 메모리에 동시에 접근하는 것을 막을 수 있다.

- 실무 코드에서는 **채널**이라는 방법을 사용할 것이다. 채널은 고루틴이 메모리를 공유하는 용도로 만들어진 일종의 큐이다. 따라서 고루틴이 각기 다른 실행흐름 가운데 값을 만들어낸다 하여도 채널을 통해 밀어넣은 값은 **동기화가 보장된다.**

- 채널은 디폴트로 상대방이 준비된 후 값을 주고받기 때문에, 별도의 동기화 과정이나 condition variable 설정 없이 goroutine을 쓸 수 있습니다.
