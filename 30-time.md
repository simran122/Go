# Time in Go (`time` package)

**← [Back to INDEX](INDEX.md)**

The **`time`** package lets you get the **current time**, **wait** (sleep), **format** dates, and **parse** date strings. Everything is in simple language.

---

## Get current time

**`time.Now()`** returns the **current date and time** (type **`time.Time`**).

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    now := time.Now()
    fmt.Println(now)  // e.g. 2025-01-31 14:30:00.123456789 +0530 IST
}
```

---

## Sleep (wait for some time)

**`time.Sleep(duration)`** pauses your program for that **duration**. Use it when you want to wait (e.g. in examples or tests).

```go
time.Sleep(2 * time.Second)   // wait 2 seconds
time.Sleep(500 * time.Millisecond)  // wait 500 milliseconds
```

- **`time.Second`** = 1 second.  
- **`time.Millisecond`** = 1 millisecond.  
- **`2 * time.Second`** = 2 seconds.

---

## Format time (Time → string)

**`t.Format(layout)`** turns a **`time.Time`** into a **string** using a **layout**. The layout is a **reference time**: **Mon Jan 2 15:04:05 MST 2006**. You change the format by writing that date in the shape you want.

**Simple rule:** Use this exact reference: **2** for day, **1** for month, **6** for year (2006), **15** for hour (24h), **04** for minute, **05** for second.

```go
now := time.Now()
fmt.Println(now.Format("2006-01-02"))              // 2025-01-31
fmt.Println(now.Format("02-Jan-2006"))             // 31-Jan-2025
fmt.Println(now.Format("15:04:05"))                // 14:30:00
fmt.Println(now.Format("2006-01-02 15:04:05"))    // 2025-01-31 14:30:00
```

**Common layouts:**

| Layout | Example output |
|--------|-----------------|
| **`"2006-01-02"`** | 2025-01-31 |
| **`"15:04:05"`** | 14:30:00 |
| **`"2006-01-02 15:04:05"`** | 2025-01-31 14:30:00 |
| **`time.RFC3339`** | 2025-01-31T14:30:00+05:30 |

---

## Parse time (string → Time)

**`time.Parse(layout, str)`** turns a **string** into a **`time.Time`**. You must use the **same layout** as the string format. It returns **`(time.Time, error)`** – always check the error.

```go
t, err := time.Parse("2006-01-02", "2025-01-31")
if err != nil {
    panic(err)
}
fmt.Println(t)  // 2025-01-31 00:00:00 +0000 UTC
```

---

## Duration (how long something takes)

**`time.Duration`** is a type for a **length of time** (e.g. 2 seconds, 100 ms). You can add/subtract durations and compare them.

```go
d := 3 * time.Second
fmt.Println(d)  // 3s

start := time.Now()
time.Sleep(1 * time.Second)
elapsed := time.Since(start)  // how long since start
fmt.Println(elapsed)  // ~1s
```

- **`time.Since(t)`** = **`time.Now().Sub(t)`** = how much time has passed since **t**.

---

## Summary

| Task | Function |
|------|----------|
| Current time | **`time.Now()`** |
| Sleep | **`time.Sleep(duration)`** |
| Format (Time → string) | **`t.Format("2006-01-02 15:04:05")`** |
| Parse (string → Time) | **`time.Parse(layout, str)`** |
| Duration | **`time.Second`**, **`time.Millisecond`**, **`time.Since(t)`** |

**← [Back to INDEX](INDEX.md)** | Next: [31-encoding-base64.md](31-encoding-base64.md) – **Base64**: encode and decode.
