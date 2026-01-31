# Methods in Go

**← [Back to INDEX](INDEX.md)**

A **method** is a **function** that is attached to a **type** (usually a struct). You call it with **`value.MethodName()`** or **`pointer.MethodName()`**. Methods let you group behavior with the data it uses.

---

## Why use methods?

- They keep **behavior** (functions) close to **data** (the type).
- They make calls readable: **`person.Greet()`** instead of **`greet(person)`**.
- They support **interfaces** (a type can satisfy an interface by having the right methods).

---

## Defining a method

You add an extra **receiver** between **`func`** and the method name. The receiver is the value (or pointer) the method is called on.

**Value receiver (works on a copy):**

```go
package main

import "fmt"

type Rectangle struct {
    Width  float64
    Height float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

func main() {
    rect := Rectangle{Width: 10, Height: 5}
    fmt.Println(rect.Area())  // 50
}
```

Here **`(r Rectangle)`** is the receiver. **`Area()`** is the method. You call it as **`rect.Area()`**.

---

## Value receiver vs pointer receiver

- **Value receiver:** **`func (r Rectangle) Area() ...`**  
  The method gets a **copy** of the struct. Changes inside the method do **not** affect the original.

- **Pointer receiver:** **`func (r *Rectangle) Scale(f float64)`**  
  The method gets a **pointer** to the struct. Changes **do** affect the original.

**When to use a pointer receiver:**

- When the method must **change** the receiver.
- When the struct is large, to avoid copying.

```go
func (r *Rectangle) Scale(factor float64) {
    r.Width *= factor
    r.Height *= factor
}

func main() {
    rect := Rectangle{Width: 10, Height: 5}
    rect.Scale(2)  // Go allows this: rect is automatically passed as &rect
    fmt.Println(rect.Width, rect.Height)  // 20 10
}
```

Go lets you call **`rect.Scale(2)`** even when **`Scale`** has a pointer receiver; Go passes **`&rect`** for you.

---

## Method on a pointer

If the receiver is **`*Rectangle`**, the method can modify the receiver. If the receiver is **`Rectangle`**, it cannot.

```go
func (r Rectangle) Area() float64 {
    return r.Width * r.Height  // read-only, no copy of pointer needed
}

func (r *Rectangle) Scale(factor float64) {
    r.Width *= factor
    r.Height *= factor  // must use pointer to change fields
}
```

---

## Summary

| Idea | Example |
|------|---------|
| Value receiver | `func (r Rectangle) Area() float64 { }` |
| Pointer receiver | `func (r *Rectangle) Scale(f float64) { }` |
| Call | `rect.Area()`, `rect.Scale(2)` |

Use a **pointer receiver** when the method needs to change the receiver or when the type is large.

**← [Back to INDEX](INDEX.md)** | Next: [14-interfaces.md](14-interfaces.md) – **Interfaces**: define behavior without fixing the type.
