# Reflection in Go (reflect package)

**← [Back to INDEX](INDEX.md)**

**Reflection** lets you inspect and work with **types and values at runtime** – for example, list the fields of a struct or get the type of a variable. In Go this is done with the **`reflect`** package. It is **advanced** and used by libraries (e.g. JSON encoding) more than in everyday code. This page gives a short, simple overview.

**Concepts used in this page:** We use **structs** ([11-structs.md](11-structs.md)), **interfaces** ([14-interfaces.md](14-interfaces.md)), and **pointers** ([12-pointers.md](12-pointers.md)). Read those first if you haven’t.

---

## Why use reflection?

- **Generic behavior** – when you don’t know the type at compile time (e.g. “print all fields of any struct”).
- **Libraries** – **encoding/json**, **encoding/xml**, and similar packages use reflection to work with arbitrary structs.
- **Avoid** reflection when you can – it is slower and less type-safe than normal code. Prefer interfaces and generics when they are enough.

---

## Type and Value

**`reflect.TypeOf(x)`** – type of **x** (e.g. **int**, **string**, **struct**).  
**`reflect.ValueOf(x)`** – the **value** of **x** as a **reflect.Value**; you can inspect or change it (if it’s settable).

```go
package main

import (
    "fmt"
    "reflect"
)

func main() {
    x := 42
    t := reflect.TypeOf(x)
    v := reflect.ValueOf(x)
    fmt.Println(t)   // int
    fmt.Println(v)  // 42
    fmt.Println(v.Kind())  // int
}
```

**Kind** is the “category” of type: **int**, **string**, **struct**, **slice**, **map**, etc.

---

## Inspecting a struct

You can list the **fields** of a struct and their types and values.

```go
package main

import (
    "fmt"
    "reflect"
)

type Person struct {
    Name string
    Age  int
}

func main() {
    p := Person{Name: "Alice", Age: 30}
    v := reflect.ValueOf(p)
    typ := v.Type()
    for i := 0; i < v.NumField(); i++ {
        field := typ.Field(i)
        val := v.Field(i)
        fmt.Printf("%s: %v (%s)\n", field.Name, val.Interface(), field.Type)
    }
}
// Name: Alice (string)
// Age: 30 (int)
```

**NumField()**, **Field(i)**, **Type().Field(i)** – number of fields, value of field **i**, and name/type of field **i**.

---

## Setting a value (must be settable)

To **set** a value through reflection, you need a **pointer** and then **Elem()** so the reflection value is settable.

```go
x := 42
v := reflect.ValueOf(&x).Elem()  // pointer so we can set
v.SetInt(99)
fmt.Println(x)  // 99
```

If you pass **x** (not **&x**), the **Value** is not settable and **SetInt** would panic.

---

## When to use reflection

- **Libraries** that must work with arbitrary types (JSON, XML, ORMs, etc.).
- **Debugging** or **logging** that prints struct fields generically.
- **Avoid** in hot paths or when interfaces or generics can do the job.

---

## Summary

| Idea | Example |
|------|---------|
| Type of **x** | **`reflect.TypeOf(x)`** |
| Value of **x** | **`reflect.ValueOf(x)`** |
| Kind | **`v.Kind()`** – int, string, struct, slice, map, etc. |
| Struct fields | **`v.NumField()`**, **`v.Field(i)`**, **`typ.Field(i).Name`** |
| Set value | Pass **pointer**, then **`.Elem()`**; use **`v.SetInt(...)`** etc. |

Reflection is **advanced**; use it when you need runtime type information that interfaces or generics cannot provide.

**← [Back to INDEX](INDEX.md)** | Prev: [46-benchmarking.md](46-benchmarking.md) | Next: [48-cgo.md](48-cgo.md) – **cgo**.
