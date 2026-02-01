# Closures in Go

**← [Back to INDEX](INDEX.md)**

A **closure** is a function that **remembers** variables from the place where it was created. Even after that place has finished, the function can still use those variables. In Go, anonymous functions (functions without a name) can be closures when they use variables from the outer scope.

**Concepts used in this page:** We mention **goroutines** ([17-goroutines.md](17-goroutines.md)) and **defer** ([19-defer-panic-recover.md](19-defer-panic-recover.md)) in examples. Read those first if you need them.

---

## What is a closure?

Think of it like a function that "closes over" some variables: it **captures** them and keeps them alive. When you call the function later, it still sees the **current** value of those variables (or the value they had when the closure was created, depending on when you use them).

---

## A simple closure

```go
package main

import "fmt"

func main() {
    count := 0
    // increment is a closure: it "captures" count
    increment := func() int {
        count++
        return count
    }
    fmt.Println(increment())  // 1
    fmt.Println(increment())  // 2
    fmt.Println(increment())  // 3
}
```

Here **`increment`** is an **anonymous function** that uses **`count`** from the outer scope. Each call updates and returns **`count`**, so the closure "remembers" **`count`** between calls.

---

## Returning a closure

A function can **return** a closure. The returned function keeps access to variables from the outer function.

```go
package main

import "fmt"

// makeCounter returns a function that counts from 0 each time you call makeCounter
func makeCounter() func() int {
    n := 0
    return func() int {
        n++
        return n
    }
}

func main() {
    counter1 := makeCounter()
    counter2 := makeCounter()
    fmt.Println(counter1())  // 1
    fmt.Println(counter1())  // 2
    fmt.Println(counter2())  // 1  (different n)
    fmt.Println(counter1())  // 3
}
```

Each call to **`makeCounter()`** creates a **new** **`n`** and a new closure. So **`counter1`** and **`counter2`** each have their own **`n`**.

---

## Closures in loops (careful)

If you create closures inside a **loop** and they all capture the **same** loop variable, they all see the **final** value of that variable when they run (e.g. in a goroutine). To fix that, pass the loop value as an argument or create a copy inside the loop.

```go
package main

import "fmt"

func main() {
    // BAD: all closures see the same i (e.g. 3 at the end)
    var badFns []func() int
    for i := 0; i < 3; i++ {
        badFns = append(badFns, func() int { return i })
    }
    fmt.Println(badFns[0](), badFns[1](), badFns[2]())  // 3 3 3

    // GOOD: each closure gets its own copy
    var goodFns []func() int
    for i := 0; i < 3; i++ {
        i := i  // copy for this iteration
        goodFns = append(goodFns, func() int { return i })
    }
    fmt.Println(goodFns[0](), goodFns[1](), goodFns[2]())  // 0 1 2
}
```

So: when you use a loop variable inside a closure (or a goroutine), **copy it** (e.g. **`i := i`**) so each closure has its own value.

---

## Where you already see closures

- **Goroutines** – `go func() { ... use outer variable ... }()` often uses a closure (see [17-goroutines.md](17-goroutines.md)).
- **`defer`** – `defer func() { ... }()` can close over variables.
- **Callbacks** – passing a `func()` that uses variables from the caller.

---

## Summary

| Idea | Example |
|------|---------|
| Closure | A function that uses variables from the outer scope; it "captures" them |
| Anonymous function | `func() { ... }` or `func(x int) int { return x+1 }` |
| Returning a closure | `func makeCounter() func() int { n:=0; return func() int { n++; return n } }` |
| Loop + closure | Copy loop variable: `i := i` before using in closure/goroutine |

**← [Back to INDEX](INDEX.md)** | Prev: [41-context.md](41-context.md) | Next: [43-runes-unicode.md](43-runes-unicode.md) – **Runes and Unicode**.
