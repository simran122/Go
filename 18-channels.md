# Channels in Go

**← [Back to INDEX](INDEX.md)**

A **channel** is a way for **goroutines** to **send** and **receive** values. One goroutine sends a value into the channel; another goroutine receives it. Channels help you **synchronize** and **pass data** between concurrent code.

---

## Why use channels?

- **Pass data** between goroutines (e.g. send results, tasks, or signals).
- **Synchronize** – e.g. one goroutine waits until another sends a value.
- **Safe** – Go makes sure only one goroutine uses a value at a time when it is sent/received.

---

## Create a channel

Use **`make(chan type)`** for an **unbuffered** channel, or **`make(chan type, size)`** for a **buffered** channel.

```go
ch := make(chan int)        // unbuffered channel of int
chBuffered := make(chan int, 10)  // buffered: can hold 10 ints
```

**Unbuffered:** A **send** blocks until someone **receives**. So sender and receiver “meet” at the channel.  
**Buffered:** Sends don’t block until the buffer is **full**; receives don’t block until the buffer is **empty**.

---

## Send and receive

- **Send:** **`channel <- value`** (e.g. **`ch <- 42`**).
- **Receive:** **`value := <-channel`** (e.g. **`x := <-ch`**).

The **arrow** shows the direction: **<-** means “value goes into the channel” when on the right of **channel**, and “value comes out” when on the left.

```go
package main

import "fmt"

func main() {
    ch := make(chan int)
    go func() {
        ch <- 42  // send 42 into ch
    }()
    value := <-ch  // receive from ch (blocks until something is sent)
    fmt.Println(value)  // 42
}
```

Here the **main** goroutine **receives** and blocks until the other goroutine **sends** 42.

---

## Unbuffered channel = synchronization

With an **unbuffered** channel, a **send** waits until a **receive** happens, and a **receive** waits until a **send** happens. So you use it to “wait until the other goroutine is done” or “wait until I get a value.”

```go
ch := make(chan int)
go func() {
    doWork()
    ch <- 1  // signal: "I'm done" (value doesn't matter)
}()
<-ch  // wait for the signal
fmt.Println("Work is done")
```

---

## Buffered channel

A **buffered** channel can hold a fixed number of values. Sends **don’t block** until the buffer is full; receives **don’t block** until the buffer is empty.

```go
ch := make(chan int, 2)
ch <- 1  // OK, buffer has space
ch <- 2  // OK, buffer has space
// ch <- 3  // would block here – buffer full (if no one receives)
fmt.Println(<-ch)  // 1
fmt.Println(<-ch)  // 2
```

---

## Close a channel

The **sender** can **close** a channel when no more values will be sent: **`close(ch)`**. Receivers can then detect “no more values” by using the two-value form of receive:

**`value, ok := <-ch`**  
If **ok** is **false**, the channel is closed and no more values will come.

```go
ch := make(chan int)
go func() {
    ch <- 1
    ch <- 2
    close(ch)
}()
for {
    v, ok := <-ch
    if !ok {
        break  // channel closed
    }
    fmt.Println(v)
}
```

**Rule:** Only the **sender** should close the channel. Don’t send after closing.

---

## Range over a channel

**`for value := range ch`** receives values until the channel is **closed**. Then the loop exits.

```go
for v := range ch {
    fmt.Println(v)
}
```

So the sender must **close(ch)** when done, or this loop never ends.

---

## Select: wait on multiple channels

**`select`** is like a **switch** for channels: it waits until **one** of the channel operations (send or receive) can run. You use it when you want to listen to **more than one channel** at once.

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)
    go func() {
        time.Sleep(100 * time.Millisecond)
        ch1 <- "first"
    }()
    go func() {
        time.Sleep(200 * time.Millisecond)
        ch2 <- "second"
    }()
    for i := 0; i < 2; i++ {
        select {
        case v := <-ch1:
            fmt.Println("Got", v)
        case v := <-ch2:
            fmt.Println("Got", v)
        }
    }
}
```

- **`select`** blocks until **one** of the **case**s can run (e.g. a value is ready on **ch1** or **ch2**).
- **`case v := <-ch`** – receive from channel; run this branch when a value is ready.
- **`case ch <- value`** – send to channel; run this branch when the channel can take the value.
- **`default`** – run immediately if no other case is ready (non-blocking).

So **select** lets you wait on **multiple channels** and react to whichever is ready first.

---

## Summary: channel topics covered

| Topic | What you learned |
|-------|-------------------|
| Create | `make(chan int)` (unbuffered) or `make(chan int, 10)` (buffered) |
| Send | `ch <- value` |
| Receive | `value := <-ch` or `value, ok := <-ch` (ok is false when closed) |
| Unbuffered | Send blocks until receive; receive blocks until send (synchronization) |
| Buffered | Send blocks only when full; receive blocks only when empty |
| Close | `close(ch)` – only sender closes; no send after close |
| Range | `for v := range ch { }` – receives until channel is closed |
| Select | `select { case v := <-ch: ... }` – wait on multiple channels |

These are the main channel topics. Channels are the main way to **communicate** and **synchronize** between goroutines in Go.

**← [Back to INDEX](INDEX.md)** | Next: [19-defer-panic-recover.md](19-defer-panic-recover.md) – **Defer, Panic, and Recover**.
