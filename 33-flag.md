# Command-line Flags in Go (`flag` package)

**← [Back to INDEX](INDEX.md)**

The **`flag`** package lets your program accept **options** from the command line (e.g. **`-port 8080`**, **`-name Alice`**). Everything is in simple language.

**Concepts used in this page:** We use **pointers** ([12-pointers.md](12-pointers.md)) – flag functions return pointers to the values. Read that first if you haven’t.

---

## Why use flags?

So users can **pass options** when they run your program instead of hard-coding values. For example: **`myapp -port 3000 -debug`** instead of changing code and recompiling.

---

## Define and parse flags

**Step 1:** Define flags with **`flag.Int`**, **`flag.String`**, **`flag.Bool`**, etc. Each returns a **pointer** to the value.

**Step 2:** Call **`flag.Parse()`** so Go reads the command line. Call **Parse** **after** defining all flags.

```go
package main

import (
    "flag"
    "fmt"
)

func main() {
    port := flag.Int("port", 8080, "port number")
    name := flag.String("name", "guest", "user name")
    debug := flag.Bool("debug", false, "enable debug mode")
    flag.Parse()

    fmt.Println("Port:", *port)
    fmt.Println("Name:", *name)
    fmt.Println("Debug:", *debug)
}
```

**Run:** `go run main.go -port 3000 -name Bob -debug`

- **`flag.Int("port", 8080, "help")`** – int flag named **port**, default **8080**, help text **"port number"**.
- **`flag.String("name", "guest", "help")`** – string flag named **name**, default **"guest"**.
- **`flag.Bool("debug", false, "help")`** – bool flag; use **`-debug`** to set **true**.
- **`flag.Parse()`** – reads **os.Args** and fills the flag values. Must be called **after** defining flags.
- You get **pointers** (e.g. **`*port`**) so you use **`*port`** to get the value.

---

## All flag types

| Function | Type | Example |
|----------|------|---------|
| **`flag.Int(name, default, help)`** | int | **`-port 8080`** |
| **`flag.Int64(name, default, help)`** | int64 | **`-size 1000000`** |
| **`flag.Uint(name, default, help)`** | uint | **`-count 5`** |
| **`flag.Float64(name, default, help)`** | float64 | **`-ratio 1.5`** |
| **`flag.String(name, default, help)`** | string | **`-name Alice`** |
| **`flag.Bool(name, default, help)`** | bool | **`-debug`** (true) or omit (false) |
| **`flag.Duration(name, default, help)`** | time.Duration | **`-timeout 5s`** |

---

## Using a variable (FlagSet style)

You can bind a flag to an **existing variable** with **`flag.IntVar`**, **`flag.StringVar`**, etc.

```go
var port int
var name string
flag.IntVar(&port, "port", 8080, "port number")
flag.StringVar(&name, "name", "guest", "user name")
flag.Parse()

fmt.Println(port, name)
```

- **`flag.IntVar(&port, "port", 8080, "help")`** – flag **-port** will write into **port**.
- You pass the **address** of the variable (**`&port`**).

---

## After Parse: non-flag arguments

After **`flag.Parse()`**, **`flag.Args()`** returns the **remaining** arguments (the ones that are not flags).

```go
flag.Parse()
args := flag.Args()
fmt.Println(args)  // e.g. ["file1.txt", "file2.txt"]
```

**Run:** `myapp -port 8080 file1.txt file2.txt` → **args** = **["file1.txt", "file2.txt"]**.

---

## Help (usage)

**`flag.Usage`** is a function that prints help. By default it shows all flags. You can set **`flag.Usage`** to your own function to customize the message.

**`flag.PrintDefaults()`** prints the default help for all flags (name, type, default, help string).

```go
flag.Usage = func() {
    fmt.Fprintf(os.Stderr, "Usage of %s:\n", os.Args[0])
    flag.PrintDefaults()
}
```

**Run:** `myapp -h` or `myapp -help` → Go calls **Usage** and exits.

---

## Summary

| Task | Function |
|------|----------|
| Int flag | **`flag.Int("name", default, "help")`** |
| String flag | **`flag.String("name", default, "help")`** |
| Bool flag | **`flag.Bool("name", default, "help")`** |
| Bind to variable | **`flag.IntVar(&var, "name", default, "help")`** |
| Parse command line | **`flag.Parse()`** (after defining flags) |
| Remaining args | **`flag.Args()`** |
| Help | **`flag.Usage`**, **`flag.PrintDefaults()`** |

**← [Back to INDEX](INDEX.md)** | Next: [34-log.md](34-log.md) – **Log**: logging and log.Fatal.
