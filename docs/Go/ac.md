# 2.0 Account + NewAccount

### public variable needs to be private ==> NEED Constructor

> accounts/accounts.go

```go
package accounts

// Account struct
type Account struct {
	Owner   string //public
	Balance int //public
}
```

> main.go


```go
package main

import (
	"./accounts"
)

func main() {
	account := accounts.Account({Owner: "mogi", Balance: 1000})
}
```

- In main program, Account Owner and Balance value can be updated from outside of the struct

### By Function ==> handling like Constructor

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
```

- *newAccount* (ERROR) , to export func should be declared as Upper case
- *NewAccount* returns **an address of the object** : `*Account` (point to Account)

> main.go


```go
package main

import (
	"./accounts"
)

func main() {
	account := accounts.NewAccount("mogi")
}
```

### constructor-like factory functions
- [constructor-like factory functions Pattern](http://www.golangpatterns.info/object-oriented/constructors)  
