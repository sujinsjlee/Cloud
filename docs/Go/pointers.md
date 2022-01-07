# 1.8 Pointers! 

- [Go - Pointers](https://go.dev/tour/moretypes/1)
    - Go has pointers. A pointer holds the memory address of a value.
    - The type `*T` is a pointer to a `T` value.
    - The & operator generates a pointer to its operand.
    - The * operator denotes the pointer's underlying value.

### Call by value - copy

```go
package main

import "fmt"

func main() {
	a := 2
	b := a
	a = 10
	fmt.Println(a, b) // 10 2
}
``` 

### Call by reference - no copy

```go
package main

import "fmt"
/*
	& : generate pointer as a memory address
	* : read through the pointer
*/
func main() {
	a := 2
	b := &a
	a = 10
	fmt.Println(a, *b) // 10 10
}
``` 

- `*` : see through, look through
- `&` : by `&`, you can access to the same object in the memory


```go
package main

import "fmt"
/*
	& : memory address
	* : access to the value of memory address
*/
func main() {
	a := 2
	b := &a // b is a pointer to a
	*b = 20 // using b, you can update a
	fmt.Println(a, *b) // 20, 20
}
``` 


### Unlike C, Go has no pointer arithmetic

> C++


- A pointer in c is an address, which is a numeric value. Therefore, you can perform arithmetic operations on a pointer just as you can on a numeric value. There are four arithmetic operators that can be used on pointers: ++, --, +, and -

```c++
#include <stdio.h>

const int MAX = 3;

int main () {

   int  var[] = {10, 100, 200};
   int  i, *ptr;

   /* let us have array address in pointer */
   ptr = var;
	
   for ( i = 0; i < MAX; i++) {

      printf("Address of var[%d] = %x\n", i, ptr );
      printf("Value of var[%d] = %d\n", i, *ptr );

      /* move to the next location */
      ptr++;
   }
	
   return 0;
}
```

- 배열이름 : 배열의 시작 주소

```
Address of var[0] = bf882b30
Value of var[0] = 10
Address of var[1] = bf882b34
Value of var[1] = 100
Address of var[2] = bf882b38
Value of var[2] = 200
```


> Go


- Go does not support pointer arithmetic which is present in other languages like C and C++.


```go
package main

func main() {  
    b := [...]int{109, 110, 111}
    p := &b // array name represents address of the first index of the array
    p++ // ERROR
}
```
- The above program will throw compilation error main.go:6: invalid operation: p++ (non-numeric type *[3]int)
