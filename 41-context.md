# Context in Go (`context` package)

**← [Back to INDEX](INDEX.md)**

The **`context`** package lets you **stop** long-running work (e.g. an HTTP request or a goroutine) or give it a **time limit**. Many Go APIs take **`context.Context`** as the first argument. Everything is in simple language.

**What is `ctx`?** **`ctx`** is the **context** – it carries a **signal to stop** (cancellation) and optionally a **timeout** or **deadline**. When you call **cancel()** (or when the time is up), **ctx** is “cancelled” and code that checks **ctx.Done()** sees it and can stop.

**What is `cancel`?** **`cancel`** is a **function** you call when you want to **stop** the work. After you call **cancel()**, the context is cancelled and any goroutine waiting on **ctx.Done()** will see it. Always call **cancel()** when you are done (e.g. **defer cancel()**).

**Concepts used in this page:** We use **goroutines** ([17-goroutines.md](17-goroutines.md)), **channels** ([18-channels.md](18-channels.md)), **defer** ([19-defer-panic-recover.md](19-defer-panic-recover.md)), and **select** ([18-channels.md](18-channels.md)). Read those first if you haven’t.

---

## Why use context?

- **Cancellation** – tell a goroutine or HTTP request to **stop** (e.g. user clicked cancel).
- **Timeout** – stop after a **duration** (e.g. 5 seconds).
- **Deadline** – stop at a **specific time**.
- **Values** – pass request-scoped values (e.g. request ID) through the call chain. (Less common for beginners.)

---

## Background and TODO

**`context.Background()`** – empty context; never cancelled. Use as **root** when you don’t have a parent context.  
**`context.TODO()`** – same as **Background**; use when you plan to add a real context later.

```go
ctx := context.Background()
// pass ctx to functions that need it
```

---

## WithCancel – cancel when you want

**`context.WithCancel(parent)`** returns a new context and a **cancel function**. When you call **cancel()**, the context is **cancelled**; goroutines that check **ctx.Done()** will see it.

```go
ctx, cancel := context.WithCancel(context.Background())
defer cancel()  // call cancel when function returns

go func() {
    select {
    case <-ctx.Done():
        fmt.Println("Cancelled")
        return
    default:
        // do work
    }
}()
// later: cancel() to stop the goroutine
```

- **`ctx.Done()`** – a **channel** that **closes** when the context is cancelled. So **`<-ctx.Done()`** means “wait until the stop signal is sent.”
- **Always** call **cancel()** when you are done (e.g. **defer cancel()**) to release resources.

---

## WithTimeout – cancel after a duration

**`context.WithTimeout(ctx, duration)`** returns a context that is **cancelled** after **duration**. Same as **WithDeadline(ctx, time.Now().Add(duration))**.

```go
ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
defer cancel()

req, err := http.NewRequestWithContext(ctx, "GET", "https://example.com", nil)
if err != nil {
    panic(err)
}
resp, err := http.DefaultClient.Do(req)
if err != nil {
    // err might be context.DeadlineExceeded if timeout
    log.Fatal(err)
}
defer resp.Body.Close()
```

- After **5 seconds**, **ctx** is cancelled and the HTTP request will be cancelled (if the client supports it).
- **context.DeadlineExceeded** – error value when context is cancelled due to timeout/deadline.

---

## WithDeadline – cancel at a time

**`context.WithDeadline(ctx, time)`** returns a context that is **cancelled** at **time** (or when parent is cancelled, whichever is first).

```go
deadline := time.Now().Add(10 * time.Second)
ctx, cancel := context.WithDeadline(context.Background(), deadline)
defer cancel()
```

---

## Check if context is cancelled

- **`ctx.Done()`** – channel that closes when context is cancelled. Use in **select** to stop work.
- **`ctx.Err()`** – returns **nil** if not cancelled; **context.Canceled** or **context.DeadlineExceeded** if cancelled.

```go
select {
case <-ctx.Done():
    fmt.Println("Stopped:", ctx.Err())
    return
default:
    // continue work
}
```

---

## HTTP with context

**`http.NewRequestWithContext(ctx, method, url, body)`** creates a request that **respects** the context: if **ctx** is cancelled (e.g. timeout), the request can be aborted.

```go
ctx, cancel := context.WithTimeout(context.Background(), 3*time.Second)
defer cancel()

req, err := http.NewRequestWithContext(ctx, "GET", "https://example.com", nil)
if err != nil {
    log.Fatal(err)
}
resp, err := http.DefaultClient.Do(req)
if err != nil {
    if errors.Is(err, context.DeadlineExceeded) {
        fmt.Println("Request timed out")
    }
    log.Fatal(err)
}
defer resp.Body.Close()
```

---

## Summary

| Task | Function / idea |
|------|------------------|
| Root context | **`context.Background()`**, **`context.TODO()`** |
| Cancel | **`context.WithCancel(ctx)`** → **ctx, cancel**; call **cancel()** |
| Timeout | **`context.WithTimeout(ctx, duration)`** |
| Deadline | **`context.WithDeadline(ctx, time)`** |
| Check cancelled | **`<-ctx.Done()`**, **`ctx.Err()`** |
| Errors | **`context.Canceled`**, **`context.DeadlineExceeded`** |
| HTTP | **`http.NewRequestWithContext(ctx, ...)`** |

**← [Back to INDEX](INDEX.md)** | Next: [42-closures.md](42-closures.md) – **Closures** | See also: [17-goroutines.md](17-goroutines.md), [35-net-http.md](35-net-http.md), [33-more-useful-packages.md](33-more-useful-packages.md)
