# 2.4 Dictionary - Understanding type

## type

> mydict/mydict.go

```go
package mydict

import "errors"

// Dictionary type
type Dictionary map[string]string

var errNotFound = errors.New("Not Found")

// Search for a word
func (d Dictionary) Search(word string) (string, error) {
	value, exists := d[word]
	if exists {
		return value, nil
	}
	return "", errNotFound
}
```

- Dictionary is **alias** for map[string]string
- **maps**
    - [Go maps](https://go.dev/blog/maps)
    - **A two-value assignment** tests for the existence of a key:
        ```go
        i, ok := m["route"]
        ```
    - In this statement, the first value (i) is assigned the value stored under the key "route". 
    - If that key doesn’t exist, i is the value type’s zero value (0). The second value (ok) is a bool that is true if the key exists in the map, and false if not.


> main.go


```go
package main
import (
	"fmt"
	"./mydict"
)

func main() {
	dictionary := mydict.Dictionary{"first": "First word"}
	definition, err := dictionary.Search("first")
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Println(definition)
	}
}
```

## Add method

> mydict/mydict.go

```go
package mydict
import "errors"
// Dictionary type
type Dictionary map[string]string

var errNotFound = errors.New("Not Found")
var errWordExists = errors.New("That word already exists")

// Search for a word
func (d Dictionary) Search(word string) (string, error) {
	value, exists := d[word]
	if exists {
		return value, nil
	}
	return "", errNotFound
}

// Add a word to the dictionary
func (d Dictionary) Add(word, def string) error {
	_, err := d.Search(word)
	switch err {
	case errNotFound:
		d[word] = def
	case nil:
		return errWordExists
	}
	return nil
}
```


> main.go


```go
package main
import (
	"fmt"
	"./mydict"
)

func main() {
	dictionary := mydict.Dictionary{}
	word := "hello"
	definition := "Greeting"
	err := dictionary.Add(word, definition)
	if err != nil {
		fmt.Println(err)
	}
	hello, _ := dictionary.Search(word)
	fmt.Println("found", word, "definition:", hello)
	err2 := dictionary.Add(word, definition)
	if err2 != nil {
		fmt.Println(err2)
	}
}
```

```console
> go run main.go
found hello definition: Greeting
That word already exists 
```

## Update Delete

> mydict/mydict.go

```go
package mydict
import "errors"
// Dictionary type
type Dictionary map[string]string

var (
	errNotFound   = errors.New("Not Found")
	errCantUpdate = errors.New("Cant update non-existing word")
	errWordExists = errors.New("That word already exists")
)

// Search for a word
func (d Dictionary) Search(word string) (string, error) {
	value, exists := d[word]
	if exists {
		return value, nil
	}
	return "", errNotFound
}
// Add a word to the dictionary
func (d Dictionary) Add(word, def string) error {
	_, err := d.Search(word)
	switch err {
	case errNotFound:
		d[word] = def
	case nil:
		return errWordExists
	}
	return nil
}

// Update a word
func (d Dictionary) Update(word, definition string) error {
	_, err := d.Search(word)
	switch err {
	case nil:
		d[word] = definition
	case errNotFound:
		return errCantUpdate
	}
	return nil
}

// Delete a word
func (d Dictionary) Delete(word string) {
	delete(d, word)
}
```

> main.go

```go
package main
import (
	"fmt"
	"./mydict"
)

func main() {
	dictionary := mydict.Dictionary{}
	baseWord := "hello"
	dictionary.Add(baseWord, "First")
	err := dictionary.Update(word, "Second")
	word, _ := dictionary.Search(baseWord)
	if err != nil {
		fmt.Println(err)
	} else {
		fmt.Println(word)
	}

	dictionary.Delete(baseWord)
	word2, err2 := dictionary.Search(baseWord)
	if err2 != nil {
		fmt.Println(err2)
	} else {
		fmt.Println(word2)
	}
}
```

```console
> go run main.go
Second
Not Found
```

- **delete function** in map
- The built in delete function removes an entry from the map:
    ```go
    delete(m, "route")
    ```
- The delete function doesn’t return anything, and will do nothing if the specified key doesn’t exist.