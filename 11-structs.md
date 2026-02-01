# Structs in Go

**← [Back to INDEX](INDEX.md)**

A **struct** (structure) is a type that **groups related data** into one block. Each piece of data has a **name** and a **type**. Think of it like a form: Name, Age, Email, etc.

**Objects in Go:** In languages like JavaScript or Java, you have “objects” with data and methods. In Go, the same idea is **structs** (the data) plus **methods** (functions that belong to that type). So: **structs + methods = “objects” in Go**. See [13-methods.md](13-methods.md) for methods. For how Go handles **class**, **extends**, **static**, **abstract**, and **interface** (coming from Java/C#), see [24-oop-in-go.md](24-oop-in-go.md).

---

## Why use structs?

Without structs you might keep separate variables: `name`, `age`, `email`. With a struct you keep them together: one **Person** with **Name**, **Age**, **Email**. That keeps code clearer and easier to pass around.

---

## Define a struct

Use **`type`** and **`struct`**, and list the **fields** inside braces.

```go
type Person struct {
    Name  string
    Age   int
    Email string
}
```

Each line is: **field name** then **type**. This defines a new type **`Person`** with three fields.

---

## Create and use a struct

**Option 1: List values in order**

```go
package main

import "fmt"

type Person struct {
    Name  string
    Age   int
    Email string
}

func main() {
    p := Person{"Alice", 25, "alice@example.com"}
    fmt.Println(p.Name, p.Age)  // Alice 25
}
```

**Option 2: Use field names (clearer and safer)**

```go
p := Person{
    Name:  "Bob",
    Age:   30,
    Email: "bob@example.com",
}
fmt.Println(p.Email)  // bob@example.com
```

**Access fields:** Use a **dot**: `p.Name`, `p.Age`, etc. You can also **assign** to them: `p.Age = 26`.

---

## Zero value of a struct

If you don’t set fields, they get their **zero values**: numbers → 0, strings → `""`, bools → false.

```go
var p Person
fmt.Println(p.Name, p.Age)  // "" 0
```

---

## Pointer to a struct

You often use **pointers** to structs so you don’t copy the whole struct. The dot works the same: **`p.Name`** even when `p` is `*Person`.

```go
p := &Person{Name: "Carol", Age: 28}
fmt.Println(p.Name)  // Carol (no need to write (*p).Name)
```

---

## Embedded structs (embedding)

You can put one struct **inside** another without giving it a field name. Then you can access the inner struct’s fields **directly** on the outer one.

```go
package main

import "fmt"

type Address struct {
    City  string
    State string
}

type Person struct {
    Name    string
    Address Address  // embedded by type name
}

func main() {
    p := Person{
        Name: "Alice",
        Address: Address{City: "NYC", State: "NY"},
    }
    fmt.Println(p.Address.City)  // NYC
}
```

Here **Address** is a named field, so we use **`p.Address.City`**. If you embed without a name (just `Address`), you’d use **`p.City`** (promotion).

---

## Summary

| Idea | Example |
|------|---------|
| Define | `type Person struct { Name string; Age int }` |
| Create | `p := Person{Name: "Alice", Age: 25}` |
| Access | `p.Name`, `p.Age` |
| Pointer | `p := &Person{...}; p.Name` |

Structs are the main way to build **custom data types** in Go.

**← [Back to INDEX](INDEX.md)** | Next: [12-pointers.md](12-pointers.md) – **Pointers**: working with memory addresses.
