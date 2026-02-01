# Goroutines in Go

**← [Back to INDEX](INDEX.md)**

A **goroutine** is a lightweight “thread” that runs a function **at the same time** as the rest of your program. You start one by putting the word **`go`** in front of a function call. That call runs **concurrently** (in the background) without blocking the caller.

---

## Why use goroutines?

- **Do many things at once** – e.g. handle many requests, or do work while waiting for I/O.
- **Lightweight** – Go can run thousands of goroutines; they use little memory.
- **Simple** – you don’t manage threads yourself; Go’s runtime schedules goroutines.

---

## Starting a goroutine

Put **`go`** in front of a function call. That call runs in a **new goroutine**; the code that called it does **not** wait for it to finish.

```go
package main

import (
    "fmt"
    "time"
)

func sayHello() {
    fmt.Println("Hello from goroutine!")
}

func main() {
    go sayHello()  // run sayHello in a new goroutine
    fmt.Println("Hello from main")
    time.Sleep(time.Second)  // wait so we see the goroutine's output
}
```

**`go sayHello()`** means: “run **sayHello** in the background and continue.” So **main** can print “Hello from main” before or after the goroutine prints.

---

## Goroutine with an anonymous function

You can start a goroutine with an **anonymous function** (function literal). The **`()`** at the end **calls** the function.

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    go func() {
        time.Sleep(500 * time.Millisecond)
        fmt.Println("Done after 500ms")
    }()  // () calls the function
    fmt.Println("Main continues")
    time.Sleep(time.Second)
}
```

---

## Be careful: main can exit before goroutines finish

When **main** ends, the **whole program** ends. So if you don’t wait (e.g. with **Sleep** or, better, with **channels** or **WaitGroup**), goroutines might not get to run or finish.

```go
go doWork()  // started
// main returns here – program may exit before doWork() runs!
```

So you usually **synchronize**: wait for goroutines to finish (e.g. with channels or **sync.WaitGroup**) before exiting **main**.

---

## Wait for goroutines: `sync.WaitGroup`

**What is `wg`?** **`wg`** is the **WaitGroup** – a counter that says “how many goroutines I am waiting for.” You **Add(1)** when you start one, each goroutine **Done()** when it finishes, and **Wait()** blocks until the count is 0 (all done).

A **WaitGroup** lets you wait until a **fixed number** of goroutines have finished. You **add** the count, each goroutine **calls Done()** when it finishes, and you **Wait()** until all are done.

```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    var wg sync.WaitGroup
    for i := 0; i < 3; i++ {
        wg.Add(1)  // one more goroutine to wait for
        go func(n int) {
            defer wg.Done()  // say "I'm done" when this goroutine exits
            fmt.Println("Goroutine", n)
        }(i)
    }
    wg.Wait()  // block until all have called Done()
    fmt.Println("All goroutines finished")
}
```

- **`wg.Add(1)`** – we are starting one more goroutine to wait for.
- **`wg.Done()`** – this goroutine is finished (same as **Add(-1)**).
- **`wg.Wait()`** – block until the count is 0 (all have called **Done**).

So you **don’t need** **time.Sleep** to wait; use **WaitGroup** (or channels) instead.

---

## Many goroutines

You can start **many** goroutines with a loop. Each call to **`go`** starts a new one.

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    for i := 0; i < 5; i++ {
        go func(n int) {
            fmt.Println("Goroutine", n)
        }(i)  // pass i so each goroutine gets its own number
    }
    time.Sleep(time.Second)
}
```

**Note:** We pass **`i`** as an argument **`(i)`** so each goroutine gets its own copy. If we used **`i`** directly inside the function without passing it, all goroutines might see the same final value of **i**.

---

## Summary: goroutine topics covered

| Topic | What you learned |
|-------|-------------------|
| Start a goroutine | `go functionName()` or `go func() { }()` |
| Run in background | Caller does not wait; it continues right away |
| Anonymous function | `go func() { ... }()` – pass arguments like `(i)` to avoid sharing the same variable |
| Main exits = program ends | So you must **wait** (e.g. **WaitGroup** or channels) before **main** returns |
| Wait for goroutines | **sync.WaitGroup**: **Add(1)**, **Done()**, **Wait()** |
| Many goroutines | Use a loop and **go** inside it; use **WaitGroup** to wait for all |

These are the main goroutine topics. To **send and receive data** between goroutines, use **channels** (next topic).

**← [Back to INDEX](INDEX.md)** | Next: [18-channels.md](18-channels.md) – **Channels**: send and receive data between goroutines.
