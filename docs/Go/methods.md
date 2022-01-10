# 2.1 Methods

> accounts/accounts.go

```go
package accounts
// Account struct
type Account struct {
	owner   string
	balance int
}
// NewAccount creates Account
func NewAccount(owner string) *Account {
	account := Account{owner: owner, balance: 0}
	return &account
}

// Deposit x amount on your account
func (a Account) Deposit(amount int) {
	a.balance += amount
}

// Balance of your account
func (a Account) Balance() int {
	return a.balance
}
```

> main.go

```go
package main

import (
	"fmt"
	"./accounts"
)

func main() {
	account := accounts.NewAccount("mogi")
	account.Deposit(10)
	fmt.Println(account.Balance()) // 0 !!!
}
```


- Go does not have classes. However, you can define methods on types.

- A method is a function with a special **receiver argument**.

- The **receiver** appears in its own argument list **between the func keyword and the method name.**
    - In this example, the `Deposit` method has a receiver of type `Account` named `a`.



## Pointer receivers

> accounts/accounts.go

```go
package accounts
// Account struct
type Account struct {
	owner   string
	balance int
}
// NewAccount creates Account
func NewAccount(owner string) *Account {
	account := Account{owner: owner, balance: 0}
	return &account
}

// Deposit x amount on your account
func (a *Account) Deposit(amount int) { // Pointer Receiver
	a.balance += amount
}

// Balance of your account
func (a Account) Balance() int {  // object 'a' is a copy
	return a.balance
}
```

> main.go

```go
package main

import (
	"fmt"
	"./accounts"
)

func main() {
	account := accounts.NewAccount("mogi")
	account.Deposit(10)
	fmt.Println(account.Balance()) // 10 !!!
}
```

- You can declare methods with **pointer receivers**. This means the receiver type has the literal syntax `*T`

- Methods with **pointer receivers** can modify the value to which the receiver points

## ERROR Handling


> accounts/accounts.go

```go
package accounts

import "errors"

// Account struct
type Account struct {
	owner   string
	balance int
}

var errNoMoney = errors.New("Can't withdraw")

// NewAccount creates Account
func NewAccount(owner string) *Account {
	account := Account{owner: owner, balance: 0}
	return &account
}

// Deposit x amount on your account
func (a *Account) Deposit(amount int) { 
	a.balance += amount
}

// Balance of your account
func (a Account) Balance() int {  
	return a.balance
}

// Withdraw x amount from your account
func (a *Account) Withdraw(amount int) error { // return error
	if a.balance < amount {
		// return errors.New("Can't withdraw") -> also possible way to print error
		return errNoMoney
	}
	a.balance -= amount
	return nil
}
```

> main.go

```go
package main

import (
	"fmt"
	"./accounts"
)

func main() {
	account := accounts.NewAccount("mogi")
	account.Deposit(10)
	fmt.Println(account.Balance()) // 10
    err := account.Withdraw(20)
	if err != nil {
		fmt.Println(err)
	}
	fmt.Println(account.Balance())
}
```

- [Golang Error Example](https://gobyexample.com/errors)
- There is nothing like `try-except` `try-catch` exception handling in Go
- A **nil** value in the error position indicates that there was no error.
- **you should always check error code manually in Go**

## Pretty print struct variable in Go

> accounts/accounts.go

```go
package accounts

// Account struct
type Account struct {
	owner   string
	balance int
}

// NewAccount creates Account
func NewAccount(owner string) *Account {
	account := Account{owner: owner, balance: 0}
	return &account
}

// Deposit x amount on your account
func (a *Account) Deposit(amount int) { 
	a.balance += amount
}
```

> main.go

```go
package main

import (
	"fmt"
	"./accounts"
)

func main() {
	account := accounts.NewAccount("mogi")
	account.Deposit(10)
	fmt.Println(account)
}
```

```console
> go run main.go
&{mogi 10}
```


- **Override String()**

> accounts/accounts.go

```go
package accounts

// Account struct
type Account struct {
	owner   string
	balance int
}

// NewAccount creates Account
func NewAccount(owner string) *Account {
	account := Account{owner: owner, balance: 0}
	return &account
}

// Deposit x amount on your account
func (a *Account) Deposit(amount int) { 
	a.balance += amount
}

// Balance of your account
func (a Account) Balance() int {  
	return a.balance
}

// Owner of the account
func (a Account) Owner() string {
	return a.owner
}

func (a Account) String() string {
	return fmt.Sprint(a.Owner(), "'s account.\nHas: ", a.Balance())
}
```

> main.go

```go
package main

import (
	"fmt"
	"./accounts"
)

func main() {
	account := accounts.NewAccount("mogi")
	account.Deposit(10)
	fmt.Println(account)
}
```

```console
> go run main.go
mogi's account.
Has: 10
```

