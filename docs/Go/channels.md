# 3.3 Channels

- **Channels**
    - Channels are the pipes that connect concurrent goroutines
    - Channels are the way to communicates between two goroutines


> main.go

```go
package main

import (
	"fmt"
)

func main() {
	c := make(chan bool) // create channels 'c'
	people := [2]string{"Aaa", "Bbb"}
	for _, person := range people {
		go isGood(person, c)
	}
	fmt.Println(<-c) // Receive message from c
	fmt.Println(<-c) // Receive message from c
	fmt.Println(<-c) // deadlock : only two goroutines
}

func isGood(person string, c chan bool) {
	fmt.Println(person)
	c <- true // Send true to channel c
}
```

- when `isGood()` func is finished, *true* is send to channel
- `result := go isGood(person)` ==> ERROR !!! 
    - goroutines have no return value
- ?? How does gorutine send a message using a channel??
    - `c<-true`

```console
> go run main.go
Aaa
true
Bbb
true
fatal error: all goroutines are asleep - deadlock!

goroutine 1 [chan receive]:
main.main()
    /home/runner/accounts/main.go:16 +0x210
exit status 2
exit status 1
```

- Channels are a **typed pipe** through which you can send and receive values with the channel operator, `<-`.

    ```go
    ch <- v    // Send v to channel ch.
    v := <-ch  // Receive from ch, and assign value to v.
    ```

- Like maps and slices, channels must be created before use:

    ```go
    ch := make(chan int) //  Send a channel to int type of info
    ```


## Prevent deadlock
- A **deadlock** happens when a group of goroutines are waiting for each other and none of them is able to proceed.

- **runtime ERROR** : The program will get stuck on the channel send operation waiting forever for someone to read the value. Go is able to detect situations like this at runtime (*not compile time*).


> main.go

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	c := make(chan string)
	people := [5]string{"Aaa", "Bbb", "Ccc", "Ddd", "Eee"}
	for _, person := range people {
		go isGood(person, c)
	}
	for i := 0; i < len(people); i++ { // to prevent deadlock, len(people)
		fmt.Println(<-c) // Getting a message from channel :blocking operation
	}

}

func isGood(person string, c chan string) {
	time.Sleep(time.Second * 10)
	c <- person + " is good"
}
```

```console
> go run main.go
Ddd is good
Eee is good
Aaa is good
Bbb is good
Ccc is good
```

## Channel Direction [Send-Only, Receive-only Channel]

```go
// When using channels as function parameters, you can
// specify if a channel is meant to only send or receive
// values. This specificity increases the type-safety of
// the program.

package main

import "fmt"

// This `ping` function only accepts a channel for sending
// values. It would be a compile-time error to try to
// receive on this channel.
func ping(pings chan<- string, msg string) {
	pings <- msg
}

// The `pong` function accepts one channel for receives
// (`pings`) and a second for sends (`pongs`).
func pong(pings <-chan string, pongs chan<- string) {
	msg := <-pings
	pongs <- msg
}

func main() {
	pings := make(chan string, 1)
	pongs := make(chan string, 1)
	ping(pings, "passed message")
	pong(pings, pongs)
	fmt.Println(<-pongs)
}
```

```console
$ go run channel-directions.go
passed message
```


## URL Checker 

```go
package main

import (
	"errors"
	"fmt"
	"net/http"
)

type requestResult struct {
	url    string
	status string
}

var errRequestFailed = errors.New("Request failed")

func main() {
	results := make(map[string]string)
	c := make(chan requestResult)
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
		go hitURL(url, c)
	}

	for i := 0; i < len(urls); i++ {
		result := <-c
		results[result.url] = result.status
	}

	for url, status := range results {
		fmt.Println(url, status)
	}

}

func hitURL(url string, c chan<- requestResult) {  //send-only channel c
	resp, err := http.Get(url)
	status := "OK"
	if err != nil || resp.StatusCode >= 400 {
		status = "FAILED"
	}
	c <- requestResult{url: url, status: status} //send result struct to channel c
}
```

- REsult is REALLY REALLY FAST!!! **COOL!!!**


```console
> go run main.go
https://www.facebook.com/ OK
https://soundcloud.com/ OK
https://academy.nomadcoders.co/ OK
https://www.instagram.com/ OK
https://www.reddit.com/ OK
https://www.airbnb.com/ OK
https://www.google.com/ OK
https://www.amazon.com/ OK
```