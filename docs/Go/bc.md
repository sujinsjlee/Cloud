# Buffer Channel

```go
package main

import "fmt"

func main() {
	// make channels with 1 size buffer
	ch := make(chan int, 1)
	// Send 1 to ch
	ch <- 1
	// Send 2 when the buffer is full
	// there is no routine to flush the buffer, resulting in a deadlock.
	ch <- 2
	fmt.Println(<-ch)
	fmt.Println(<-ch)
}
```

```console
fatal error: all goroutines are asleep - deadlock!
```



```go
package main

import "fmt"

func main() {
	// make channels with 2 size buffer
	ch := make(chan int, 2)
	// Send 1 to ch
	ch <- 1
	// Send 2 to ch
	ch <- 2
	fmt.Println(<-ch)
	fmt.Println(<-ch)
}
```

```console
1
2
```


- **Channels can be buffered.** 
    - Provide the buffer length as the second argument to make to initialize a buffered channel:

    ```go
    ch := make(chan int, 100)
    ```

    - Sends to a buffered channel block only when the buffer is full. Receives block when the buffer is empty.
    - 이렇게 하면 buffered 채널은 버퍼가 꽉 찰 때까지 블락되거나(값을 전송할 때), 버퍼가 다 빌 때까지 블락됩니다(값을 전달받을 때).

