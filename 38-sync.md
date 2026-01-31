# Sync in Go (`sync` package)

**← [Back to INDEX](INDEX.md)**

The **`sync`** package helps you **coordinate** goroutines (make them work together safely): **Mutex** (lock) for safe access to shared data, and **WaitGroup** for waiting until goroutines finish. Everything is in simple language.

**Concepts used in this page:** We use **goroutines** ([17-goroutines.md](17-goroutines.md)) and **defer** ([19-defer-panic-recover.md](19-defer-panic-recover.md)). Read those first if you haven’t.

---

## Why use sync?

- **Mutex** – when **more than one goroutine** reads or writes the **same variable**, only one should do it at a time. A **lock** ensures that.
- **WaitGroup** – when you start **several goroutines** and need to **wait** until all of them finish before continuing.

---

## Mutex (lock)

**`sync.Mutex`** has two methods: **`Lock()`** and **`Unlock()`**. Only one goroutine can hold the lock at a time. Others **block** until the lock is released.

```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    var mu sync.Mutex
    count := 0

    mu.Lock()
    count++
    mu.Unlock()

    mu.Lock()
    fmt.Println(count)
    mu.Unlock()
}
```

- **`mu.Lock()`** – take the lock. If another goroutine holds it, wait.
- **`mu.Unlock()`** – release the lock. **Always** call **Unlock** (often with **defer mu.Unlock()** right after **Lock()**).

---

## Mutex with goroutines

When **many goroutines** update the same variable, wrap the update in **Lock** / **Unlock**.

```go
var mu sync.Mutex
var count int

for i := 0; i < 100; i++ {
    go func() {
        mu.Lock()
        count++
        mu.Unlock()
    }()
}
// wait for goroutines (e.g. WaitGroup or time.Sleep), then read count
```

- Without the lock, **count** could be wrong (race condition). With the lock, only one goroutine updates at a time.

---

## defer Unlock

Use **defer** so you never forget **Unlock**, even if you return or panic.

```go
mu.Lock()
defer mu.Unlock()
// ... use shared data ...
return  // Unlock runs here
```

---

## WaitGroup (wait for goroutines)

**`sync.WaitGroup`** has **Add(n)**, **Done()**, and **Wait()**. You **Add** the number of goroutines you will start, each goroutine calls **Done()** when finished, and **Wait()** blocks until the count is 0.

```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    var wg sync.WaitGroup
    for i := 0; i < 3; i++ {
        wg.Add(1)
        go func(n int) {
            defer wg.Done()
            fmt.Println("Goroutine", n)
        }(i)
    }
    wg.Wait()
    fmt.Println("All done")
}
```

- **`wg.Add(1)`** – one more goroutine to wait for. Call **before** starting the goroutine (or inside it if you prefer).
- **`wg.Done()`** – same as **wg.Add(-1)**. Call when the goroutine is finished. **defer wg.Done()** is common.
- **`wg.Wait()`** – block until the count is 0 (all have called **Done**).

You also saw **WaitGroup** in [17-goroutines.md](17-goroutines.md).

---

## Once (run only once)

**`sync.Once`** has **Do(f func())**. **f** runs only **once**, no matter how many times **Do** is called. Useful for one-time setup.

```go
var once sync.Once
once.Do(func() {
    fmt.Println("This runs only once")
})
once.Do(func() {
    fmt.Println("This never runs")
})
```

---

## Summary

| Type / method | What it does |
|---------------|----------------|
| **Mutex** | **`mu.Lock()`**, **`mu.Unlock()`** – only one goroutine at a time |
| **WaitGroup** | **`wg.Add(1)`**, **`wg.Done()`**, **`wg.Wait()`** – wait for N goroutines |
| **Once** | **`once.Do(f)`** – run **f** only once |

**← [Back to INDEX](INDEX.md)** | Next: [39-fmt.md](39-fmt.md) – **fmt**: Printf, Sprintf, Scanf, Errorf.
