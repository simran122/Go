# Error Handling in Go

**← [Back to INDEX](INDEX.md)**

In Go, **errors** are values. Functions that can fail often return **two values**: the result and an **error**. You check the error and handle it. There are no exceptions like in some other languages.

---

## Why this style?

- **Explicit** – you see in the code that a function can fail and where you handle it.
- **Simple** – no try/catch; you use **if** and **return**.
- **Predictable** – control flow is clear.

---

## Returning an error

A function that can fail usually returns **`(result, error)`**. If something went wrong, it returns a non-nil **error**; if everything is fine, it returns **`nil`** for the error.

```go
package main

import (
    "errors"
    "fmt"
)

func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, errors.New("cannot divide by zero")
    }
    return a / b, nil
}

func main() {
    result, err := divide(10, 2)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    fmt.Println("Result:", result)  // Result: 5
}
```

**`errors.New("message")`** creates a simple error. **`nil`** means “no error.”

---

## Checking the error

After calling a function that returns an error, **always check** it. The usual pattern:

```go
result, err := someFunction()
if err != nil {
    // handle error: log it, return it, or exit
    return err
}
// use result
```

If you **ignore** the error (don’t check **err**), a failure can go unnoticed. So: **check every error**.

---

## Handling the error

Common ways to handle:

**1. Return the error to the caller:**

```go
data, err := readFile("config.txt")
if err != nil {
    return err  // let the caller handle it
}
```

**2. Log and exit (e.g. in main):**

```go
if err != nil {
    log.Fatal(err)  // prints error and exits program
}
```

**3. Log and continue (or use a default):**

```go
if err != nil {
    log.Println("warning:", err)
    // use default value or skip
}
```

---

## Wrapping errors with more context

You can **wrap** an error to add context (e.g. what you were doing). Use **`fmt.Errorf`** with **`%w`**:

```go
data, err := readFile(name)
if err != nil {
    return nil, fmt.Errorf("failed to read %s: %w", name, err)
}
```

**`%w`** wraps the original error so callers can **unwrap** it (e.g. with **`errors.Unwrap`** or **`errors.Is`**).

---

## Creating errors

**Simple error:**

```go
err := errors.New("something went wrong")
```

**With formatting:**

```go
err := fmt.Errorf("invalid value: %d", value)
```

**Returning nil when there is no error:**

```go
func doSomething() error {
    if badCondition {
        return errors.New("bad condition")
    }
    return nil  // success – no error
}
```

---

## Summary

| Idea | Example |
|------|---------|
| Return error | `return 0, errors.New("message")` or `return result, nil` |
| Check error | `if err != nil { ... }` |
| Create error | `errors.New("msg")` or `fmt.Errorf("msg: %w", err)` |
| Handle | Return it, log it, or log and exit (e.g. `log.Fatal(err)`) |

**Rule:** Always check **err** after calling a function that returns an error.

**← [Back to INDEX](INDEX.md)** | Next: [17-goroutines.md](17-goroutines.md) – **Goroutines**: run code at the same time (concurrency).
