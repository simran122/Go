# Pointers in Go

**← [Back to INDEX](INDEX.md)**

A **pointer** is like a **note that says where something lives** in memory. Instead of copying a whole variable, you pass or store that "address" and work through the pointer. Everything below is in simple language.

---

## Why use pointers?

- **Avoid copying** large data (e.g. big structs).
- **Change** a variable from inside another function (by passing its address).
- **Share** the same data in more than one place.

---

## Address and value

- **`&variable`** – “address of” the variable. Gives you a **pointer** to it.
- **`*pointer`** – “value at” that address. Gives you the value the pointer points to (this is called **dereferencing**).

```go
package main

import "fmt"

func main() {
    x := 10
    p := &x        // p is a pointer to x (p holds x's address)
    fmt.Println(p) // something like: 0x140000...
    fmt.Println(*p) // 10 (the value at that address)
    *p = 20        // change the value at that address
    fmt.Println(x) // 20 (x changed!)
}
```

So: **`&`** takes the address, **`*`** reads or writes the value at that address.

---

## Passing pointers to functions

You can pass a **pointer** to a function so the function can **change** the original variable. The function’s parameter type is **`*T`** (pointer to T).

```go
package main

import "fmt"

func addOne(p *int) {
    *p = *p + 1  // change the value at that address
}

func main() {
    x := 10
    addOne(&x)   // pass the address of x
    fmt.Println(x)  // 11
}
```

So: **`addOne(&x)`** passes the **address** of **x**. Inside **addOne**, **`*p`** is the value at that address, and **`*p = *p + 1`** changes **x** itself.

---

## Pointer type

The type of a pointer to an `int` is **`*int`**. So “pointer to int.”

```go
var p *int   // p is a pointer to an int
var q *string // q is a pointer to a string
```

---

## The `new` function

**`new(T)`** allocates memory for a value of type **T**, sets it to the **zero value**, and returns a **pointer** to it.

```go
package main

import "fmt"

func main() {
    p := new(int)   // p is *int, points to an int with value 0
    fmt.Println(*p) // 0
    *p = 42
    fmt.Println(*p) // 42
}
```

So **`new(int)`** gives you a **`*int`** pointing to an `int` that is `0`.

---

## Pointers and structs

When you have a **pointer to a struct**, you can still use the **dot** to access fields. Go automatically uses the value the pointer points to.

```go
type Person struct {
    Name string
    Age  int
}

p := &Person{Name: "Alice", Age: 25}
fmt.Println(p.Name)  // same as (*p).Name – Go does it for you
p.Age = 26           // same as (*p).Age = 26
```

---

## Nil pointer

A pointer that doesn’t point to anything has the value **`nil`**. Using **`*p`** when **`p`** is **`nil`** causes a **panic** (runtime error).

```go
var p *int  // p is nil
// fmt.Println(*p)  // panic!
```

So always make sure a pointer points to something (or check for **`nil`**) before dereferencing.

---

## Summary: pointer topics covered

| Topic | Code / idea |
|-------|----------------|
| Address of x | `&x` |
| Value at p (dereference) | `*p` |
| Pointer type | `*int`, `*string` |
| Create with new | `p := new(int)` |
| Pointers and structs | `p.Name` works when p is `*Person` |
| Nil pointer | `var p *int`; check before `*p` |
| Pass pointer to function | `func f(p *int)` then `f(&x)` |

These are the main pointer topics you need. For more, see the official Go docs or [INDEX.md](INDEX.md) for other topics.

**← [Back to INDEX](INDEX.md)** | Next: [13-methods.md](13-methods.md) – **Methods**: functions that belong to a type.
