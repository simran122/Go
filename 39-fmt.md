# Formatting in Go (`fmt` package)

**← [Back to INDEX](INDEX.md)**

The **`fmt`** package is used for **printing** and **formatting** text. You already use **`fmt.Println`**. This page covers **Printf**, **Sprintf**, **Scanf**, **Errorf**, and format verbs. Everything is in simple language.

---

## Why use fmt?

- **Print** – to screen (Print, Println, Printf).
- **Print to a Writer** – **Fprint**, **Fprintln**, **Fprintf** take an **io.Writer** as the first argument (e.g. file, stderr, HTTP response). Same as Print/Println/Printf, but you choose **where** the output goes. See [40-io.md](40-io.md) for **io.Writer**.
- **Format** – build a string with **%d**, **%s**, etc. (**Sprintf**).
- **Read** – from standard input (**Scanf**, **Scan**).
- **Errors** – build error messages (**Errorf**).

---

## Print functions

| Function | What it does |
|----------|----------------|
| **`fmt.Print(args...)`** | Print args; no newline (to screen) |
| **`fmt.Println(args...)`** | Print args and newline (to screen) |
| **`fmt.Printf(format, args...)`** | Print using format string (e.g. **%d**, **%s**) (to screen) |
| **`fmt.Fprint(w, args...)`** | Same as Print, but write to **w** (**io.Writer**: file, stderr, HTTP, etc.) |
| **`fmt.Fprintln(w, args...)`** | Same as Println, but write to **w** |
| **`fmt.Fprintf(w, format, args...)`** | Same as Printf, but write to **w** |

```go
fmt.Print("Hello")
fmt.Print("World\n")   // HelloWorld (or HelloWorld with newline)
fmt.Println("Hello")  // Hello + newline
fmt.Printf("Name: %s, Age: %d\n", "Alice", 25)  // Name: Alice, Age: 25
```

---

## Format verbs (placeholders)

Inside **Printf**, **Sprintf**, **Errorf**, **Scanf** you use **verbs** like **%d**, **%s**, **%v**.

| Verb | Type | Example |
|------|------|---------|
| **`%d`** | int (decimal) | 42 |
| **`%x`** | int (hex) | 2a |
| **`%f`** | float | 3.140000 |
| **`%.2f`** | float (2 decimals) | 3.14 |
| **`%s`** | string | hello |
| **`%v`** | any (default format) | 42, hello, {1 2} |
| **`%+v`** | struct (with field names) | {Name:Alice Age:25} |
| **`%#v`** | Go syntax | "hello", 42 |
| **`%t`** | bool | true |
| **`%p`** | pointer (address) | 0x140000... |
| **`%T`** | type of value | int, string |
| **`%%`** | literal % | % |

```go
name := "Alice"
age := 25
fmt.Printf("%s is %d years old\n", name, age)  // Alice is 25 years old
fmt.Printf("%v %v\n", name, age)               // Alice 25
fmt.Printf("%+v\n", struct{ N string }{N: "Hi"})  // {N:Hi}
fmt.Printf("%T\n", age)                        // int
```

---

## Sprintf (format to string)

**`fmt.Sprintf(format, args...)`** works like **Printf** but **returns a string** instead of printing.

```go
s := fmt.Sprintf("Name: %s, Age: %d", "Alice", 25)
fmt.Println(s)  // Name: Alice, Age: 25
```

- Use **Sprintf** when you need the formatted string (e.g. for **log**, **error**, or **http** response).

---

## Errorf (create an error)

**`fmt.Errorf(format, args...)`** builds an **error** with a message. Same format as **Sprintf**, but the result is **error**.

```go
err := fmt.Errorf("file not found: %s", "data.txt")
if err != nil {
    log.Println(err)  // file not found: data.txt
}
```

- Use **Errorf** when you want to **return** an error with a custom message (see [16-error-handling.md](16-error-handling.md)).

---

## Fprintf (print to a Writer)

**What is `w`?**  
**`w`** is just a **variable name**. It means **“where to write”** (people often use **w** for “writer”). It is the **first argument** of **Fprintf**: the **place** your text goes. So:

- **`fmt.Printf(...)`** → text goes to the **screen** (you don’t choose; it’s always the screen).
- **`fmt.Fprintf(w, ...)`** → text goes **wherever `w` points**: a file, the error stream, an HTTP response, etc. You pass **w** to say **where** to write.

**In short:** **w** = the destination (file, stderr, HTTP response, …). Same format string and **%d**, **%s**, etc. as **Printf**; only the **first argument** tells Go **where** to send the text.

- Use **Printf** when you want output on the **terminal**.
- Use **Fprintf** when you want output in a **file**, in **stderr**, or in an **HTTP response**.

**What can `w` be?**  
Anything that can “receive” text: a **file**, **stderr**, or an **HTTP response body**. In Go we say **w** has type **`io.Writer`** (something you can write to).

| What you pass as `w` | Where the text goes |
|---------------------|----------------------|
| **`os.Stdout`** | Terminal (same as Printf) |
| **`os.Stderr`** | Terminal, but the “error” stream (for errors/logs) |
| **`*os.File`** (e.g. from `os.Create`) | A file on disk |
| **`http.ResponseWriter`** | HTTP response (what the user sees in the browser) |

**Example: write errors to stderr**

Here **w** is **`os.Stderr`** – so the message goes to the error stream, not the normal screen output.

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    // w = os.Stderr (where to write). Message goes to stderr.
    fmt.Fprintf(os.Stderr, "Error: %s\n", "something went wrong")

    code := 404
    fmt.Fprintf(os.Stderr, "Error code %d: not found\n", code)
}
```

**Example: write to a file**

Here **w** is **`f`** (the open file). The text is written into **output.txt**.

```go
f, err := os.Create("output.txt")
if err != nil {
    fmt.Fprintf(os.Stderr, "create file: %v\n", err)
    return
}
defer f.Close()

// w = f (the file). Text goes into output.txt
fmt.Fprintf(f, "Hello, %s!\n", "world")
```

**Example: HTTP response**

Here **w** is the **HTTP response writer** – whatever you write to **w** is sent to the user’s browser.

```go
func handler(w http.ResponseWriter, r *http.Request) {
    // w = HTTP response. "Hello, Guest!" is sent to the browser.
    fmt.Fprintf(w, "Hello, %s!", "Guest")
}
```

- For more on **io.Writer** and files, see [40-io.md](40-io.md).

---

## Scan (read from input)

**`fmt.Scan(&var)`** – read one value from standard input into **var**.  
**`fmt.Scanln(&var1, &var2)`** – read until newline.  
**`fmt.Scanf(format, &vars...)`** – read using format (e.g. **"%s %d"**).

```go
var name string
var age int
fmt.Print("Enter name and age: ")
fmt.Scanln(&name, &age)
fmt.Printf("Hello, %s, age %d\n", name, age)
```

- You pass **pointers** (**&name**, **&age**) so **Scan** can write into them.

---

## Width and precision

In format strings you can set **width** and **precision**:

- **`%5d`** – int, at least 5 characters wide (padded with spaces).
- **`%5s`** – string, at least 5 characters wide.
- **`%.2f`** – float, 2 digits after decimal.

```go
fmt.Printf("%5d\n", 42)    // "   42"
fmt.Printf("%-5d\n", 42)   // "42   " (left-aligned)
fmt.Printf("%.2f\n", 3.14159)  // 3.14
```

---

## Summary

| Task | Function |
|------|----------|
| Print | **`fmt.Print`**, **`fmt.Println`**, **`fmt.Printf`** |
| Format to string | **`fmt.Sprintf(format, args...)`** |
| Create error | **`fmt.Errorf(format, args...)`** |
| Print to Writer | **`fmt.Fprintf(w, format, args...)`** |
| Read from input | **`fmt.Scan`**, **`fmt.Scanln`**, **`fmt.Scanf`** |
| Verbs | **%d**, **%s**, **%v**, **%+v**, **%f**, **%t**, **%T**, **%p** |

**← [Back to INDEX](INDEX.md)** | Next: [40-io.md](40-io.md) – **io**: Reader, Writer, ReadAll, Copy.
