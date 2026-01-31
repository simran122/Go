# Logging in Go (`log` package)

**← [Back to INDEX](INDEX.md)**

The **`log`** package prints **messages** with a timestamp (and optional prefix) and can **exit** the program on error (**log.Fatal**). Everything is in simple language.

**Concepts used in this page:** We use **defer** ([19-defer-panic-recover.md](19-defer-panic-recover.md)) and **panic** ([19-defer-panic-recover.md](19-defer-panic-recover.md)) in some examples. Read that first if you haven’t.

---

## Why use log?

- **Timestamp** – each line gets date and time (e.g. **2025/01/31 14:30:00**).
- **Prefix** – you can add an app name (e.g. **myapp: message**).
- **Fatal** – print an error and **exit** in one call, so you don’t forget to exit.

---

## Basic logging

**`log.Print(...)`** – print like **fmt.Print** (no newline unless you add **\n**).  
**`log.Println(...)`** – print like **fmt.Println** (adds newline).  
**`log.Printf(format, args...)`** – print with format like **fmt.Printf**.

```go
package main

import "log"

func main() {
    log.Println("Hello, log!")
    // 2025/01/31 14:30:00 Hello, log!

    log.Printf("User %s logged in", "Alice")
    // 2025/01/31 14:30:00 User Alice logged in
}
```

---

## Set output (where logs go)

By default logs go to **standard error** (**os.Stderr**). You can change it with **`log.SetOutput(w io.Writer)`**.

```go
import (
    "log"
    "os"
)

// Write logs to a file
f, _ := os.Create("app.log")
defer f.Close()
log.SetOutput(f)
log.Println("This goes to app.log")
```

---

## Set prefix

**`log.SetPrefix(prefix)`** adds a **prefix** to every log line. Useful to see which app or component wrote the line.

```go
log.SetPrefix("myapp: ")
log.Println("Starting server")
// myapp: 2025/01/31 14:30:00 Starting server
```

---

## Set flags (what appears in each line)

**`log.SetFlags(flags)`** controls what the logger prints: date, time, file, line, etc.

| Flag | What it adds |
|------|----------------|
| **`log.Ldate`** | Date (2009/01/23) |
| **`log.Ltime`** | Time (01:23:23) |
| **`log.Lmicroseconds`** | Microseconds |
| **`log.Llongfile`** | Full file path and line number |
| **`log.Lshortfile`** | File name and line number |
| **`log.Lmsgprefix`** | Put prefix before message instead of after time |
| **`0`** | No prefix (no date/time) |

```go
log.SetFlags(log.Ldate | log.Ltime)
log.Println("With date and time")

log.SetFlags(0)
log.SetPrefix("myapp: ")
log.Println("Only prefix, no time")
// myapp: Only prefix, no time
```

---

## Fatal: log and exit

**`log.Fatal(...)`** and **`log.Fatalf(...)`** print the message (like **Print** / **Printf**) and then call **os.Exit(1)**. Use when something is wrong and the program **must** stop.

```go
resp, err := http.Get("https://example.com")
if err != nil {
    log.Fatal(err)  // print error and exit with code 1
}
```

- **`log.Fatal(err)`** – same as **log.Print(err)** then **os.Exit(1)**.
- **Deferred functions** still run before exit.

---

## Panic: log and panic

**`log.Panic(...)`** and **`log.Panicln(...)`** print the message and then call **panic**. Use when you want a stack trace or when **panic** is part of your design (see [19-defer-panic-recover.md](19-defer-panic-recover.md)).

```go
log.Panic("something went wrong")  // prints message, then panic
```

---

## Summary

| Task | Function |
|------|----------|
| Print | **`log.Print`**, **`log.Println`**, **`log.Printf`** |
| Set prefix | **`log.SetPrefix("x: ")`** |
| Set output | **`log.SetOutput(w)`** |
| Set flags | **`log.SetFlags(flags)`** (Ldate, Ltime, Lshortfile, etc.) |
| Log and exit | **`log.Fatal(...)`**, **`log.Fatalf(...)`** |
| Log and panic | **`log.Panic(...)`** |

**← [Back to INDEX](INDEX.md)** | Next: [35-net-http.md](35-net-http.md) – **net/http**: HTTP server and client.
