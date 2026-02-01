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

- **`&variable`** – “Where does this variable live?” It gives you the **address** (a pointer) so you can find that variable later.
- **`*pointer`** – “What's stored at that address?” It gives you the **actual value** there. Following the pointer to get the value is called **dereferencing**.

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

- **`*p` on the right side** (e.g. `x := *p` or `fmt.Println(*p)`) → you're **dereferencing**: reading the value at that address.

- **`*p` on the left side** (e.g. `*p = 20`) → you're **changing** the value that the pointer points to.

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

**Why `p *int` and not `*p int`?** In Go, a parameter is always **name** then **type**. So `p` is the **name** of the parameter (the variable that holds the pointer), and **`*int`** is the **type** (pointer to int). We're declaring "a variable called `p` of type pointer-to-int." Inside the function we then use **`*p`** to dereference and read or change the value.

So: **`addOne(&x)`** passes the **address** of **x**. Inside **addOne**, **`*p`** is the value at that address, and **`*p = *p + 1`** changes **x** itself.

---

## Pointer type

The type of a pointer to an `int` is **`*int`**. So “pointer to int.”

```go
var p *int   // p is a pointer to an int
var q *string // q is a pointer to a string
```

**Why not `var x *int = 10`?** A **`*int`** holds an **address** (a pointer), not an integer. So you can't assign the number `10` to it — that would be like putting a house number inside an envelope that's meant to hold the address of the house. To have a pointer that “points to” the value 10, do one of these:

- **Option 1:** Create an int, then take its address:

  ```go
  x := 10
  var p *int = &x   // p is a pointer to x (p holds x's address)
  ```

  **Same thing, two styles:** Earlier we used `p := &x` (Go infers that `p` is `*int`). Here we wrote **`var p *int = &x`** so the type is explicit. Both are correct; use whichever you prefer.

- **Option 2:** Create a pointer with **`new`**, then set the value: `p := new(int)` then `*p = 10`.

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
