# 1.9 Arrays and Slices

- [Arrays and Slices](https://go.dev/tour/moretypes/6)

## Arrays
- An array's length is part of its type, so arrays cannot be resized.


```go
var a [10]int
```

- The type **[n]T** is an array of n values of type T.

```go
package main

import "fmt"

func main() {
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)

	primes := [6]int{2, 3, 5, 7, 11, 13}
	fmt.Println(primes)
}
```

```
Hello World
[Hello World]
[2 3 5 7 11 13]
```

## Slices
- An array has a fixed size. A slice, on the other hand, is a dynamically-sized, flexible view into the elements of an array. 

- The type **[]T** is a slice with elements of type T.

- The zero value of a slice is **nil**.

```go
package main

import "fmt"

func main() {
	primes := [6]int{2, 3, 5, 7, 11, 13}

	var s []int = primes[1:4]
	fmt.Println(s)

	var s []int
	if s == nill {
		fmt.Println("nil!")
	}
}

```

```
[3 5 7]
```

- **a[low : high]** 
- a[idx : length from idx] 
    - A slice is formed by specifying two indices, a low and high bound, separated by a colon :
    - This selects a half-open range which includes the first element(low idx), but **excludes the last one(high idx).**

- 슬라이스[복사할 첫 인덱스:복사할 마지막 인덱스+1]
    - 예를 들어 l := sliceA[2:5]라고 한다면 슬라이스 l에 sliceA의 인덱스 2요소부터 4요소까지 잘라서 복사한다는 것입니다. 마지막 요소는 복사하지 않습니다. 
    - 그리고 처음과 마지막 인덱스를 생략하면 첫 요소와 맨 마지막 요소를 의미합니다. 예를 들어 l := sliceA[:5]라면 sliceA의 처음부터 인덱스 4의 요소까지 복사한다는 것입니다. 반대의 경우도 마찬가지로 쓸 수 있습니다.

- When slicing, you may omit the high or low bounds to use their defaults instead.

- The default is zero for the low bound and the length of the slice for the high bound.

- For the array, **var a [10]int**, these slice expressions are equivalent:

```
a[0:10]
a[:10]
a[0:]
a[:]
```

### Slices - append
- It is common to append new elements to a slice

```go
package main

import "fmt"

func main() {
	var s []int
	printSlice(s)

	// append works on nil slices.
	// append(s, 0) ERROR because append has a return value

	s = append(s, 0)
	printSlice(s)

	// The slice grows as needed.
	s = append(s, 1)
	printSlice(s)

	// We can add more than one element at a time.
	s = append(s, 2, 3, 4)
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

```
len=0 cap=0 []
len=1 cap=1 [0]
len=2 cap=2 [0 1]
len=5 cap=6 [0 1 2 3 4]
```


### Slices are like references to arrays

```go
package main

import "fmt"

func main() {
	names := [4]string{
		"John",
		"Paul",
		"George",
		"Ringo",
	}
	fmt.Println(names)

	a := names[0:2]
	b := names[1:3]
	fmt.Println(a, b)

	b[0] = "XXX"
	fmt.Println(a, b)
	fmt.Println(names)
}
```


```
[John Paul George Ringo]
[John Paul] [Paul George]
[John XXX] [XXX George]
[John XXX George Ringo]
```

### Slice length and capacity

> **length** : The length of a slice is the number of elements it contains.  
> **capacity** : The capacity of a slice is the number of elements in the underlying array, **counting from the first element** in the slice.  

```go
import "fmt"

func main() {
	s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s)

	// Slice the slice to give it zero length.
	s = s[:0]
	printSlice(s)

	// Extend its length.
	s = s[:4]
	printSlice(s)

	// Drop its first two values.
	s = s[2:]
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

```
len=6 cap=6 [2 3 5 7 11 13]
len=0 cap=6 []
len=4 cap=6 [2 3 5 7]
len=2 cap=4 [5 7]
```

### Creating a slice with make

```go
package main

import "fmt"

func main() {
	a := make([]int, 5) // second argument : length
	printSlice("a", a)

	b := make([]int, 0, 5) // third argument : capacity
	printSlice("b", b)

	c := b[:2]
	printSlice("c", c)

	d := c[2:5]
	printSlice("d", d)
}

func printSlice(s string, x []int) {
	fmt.Printf("%s len=%d cap=%d %v\n",
		s, len(x), cap(x), x)
}
```

```
a len=5 cap=5 [0 0 0 0 0]
b len=0 cap=5 []
c len=2 cap=5 [0 0]
d len=3 cap=3 [0 0 0]
```