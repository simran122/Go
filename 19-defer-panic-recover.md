# Defer, Panic, and Recover in Go

**← [Back to INDEX](INDEX.md)**

Go has three built-in mechanisms that affect control flow:

- **defer** – run code when the function **returns** (cleanup, close files, etc.).
- **panic** – stop normal execution and **panic** (like a runtime error).
- **recover** – catch a **panic** and return to normal execution (only inside **defer**).

---

## Defer – run code when the function exits

**`defer`** schedules a function call to run **when the current function returns** (normally or by panic). It’s often used for **cleanup**: close files, unlock mutexes, etc.

```go
package main

import "fmt"

func main() {
    defer fmt.Println("This runs when main returns (last)")
    fmt.Println("This runs first")
    fmt.Println("This runs second")
}
```

**Output:**

```
This runs first
This runs second
This runs when main returns (last)
```

So **defer** runs in **reverse order** of how they were deferred: last deferred = first run when the function returns.

---

## Defer for cleanup

A common use: open a file, then **defer** closing it. That way the file is closed even if you return early or panic.

```go
f, err := os.Open("file.txt")
if err != nil {
    return err
}
defer f.Close()  // run when the function returns
// ... use f ...
```

---

## Panic – stop execution

**`panic`** stops the normal flow and starts **panicking**. If nothing **recovers**, the program exits (often with a stack trace). Use it for “this should never happen” or unrecoverable errors.

```go
package main

import "fmt"

func main() {
    fmt.Println("Start")
    panic("something went wrong")
    fmt.Println("End")  // never runs
}
```

**`panic(value)`** – you can pass any value (often a string or an error). When the program panics, it runs any **deferred** functions first, then exits (unless **recover** is used).

---

## Recover – catch a panic

**`recover`** is a built-in that **stops the panic** and returns the value that was passed to **panic**. It **only works inside a deferred function**. If there is no panic, **recover** returns **nil**.

```go
package main

import "fmt"

func main() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recovered:", r)
        }
    }()
    fmt.Println("Before panic")
    panic("oh no!")
    fmt.Println("After panic")  // never runs
}
```

**Output:**

```
Before panic
Recovered: oh no!
```

So you **defer** an anonymous function that calls **recover()**. If the current goroutine is panicking, **recover()** catches it and returns the panic value; execution continues after the deferred function.

---

## When to use what

| Mechanism | Use |
|-----------|-----|
| **defer** | Cleanup (close files, unlock), run code when function returns |
| **panic** | Unrecoverable or “impossible” situations; rarely in normal app logic |
| **recover** | Inside **defer**, to catch a panic and turn it into an error or log |

**Beginner tip:** Prefer **returning errors** from functions instead of **panic**. Use **panic** only when the program truly cannot continue (e.g. programming bug, missing config that must exist). Use **recover** at the “top” of a goroutine (e.g. in a deferred function) to turn a panic into a normal error.

---

## Summary

| Idea | Example |
|------|---------|
| Defer | `defer f.Close()` or `defer func() { ... }()` |
| Panic | `panic("message")` |
| Recover | `if r := recover(); r != nil { ... }` (only in defer) |

**← [Back to INDEX](INDEX.md)** | Next: [20-generics.md](20-generics.md) – **Generics**: write code that works with different types.
