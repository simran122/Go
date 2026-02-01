# Bytes in Go (`bytes` package)

**← [Back to INDEX](INDEX.md)**

The **`bytes`** package works on **`[]byte`** the way **`strings`** works on **strings**: Contains, Split, Join, Replace, Trim, etc. Use it when you have **byte slices** (e.g. from files or networks). Everything is in simple language.

---

## What is `[]byte`? (Common doubt)

- **`[]byte`** means “a **slice of bytes**”. One **byte** is a number from 0 to 255. So **`[]byte`** is a list of such numbers.
- In Go, **text** (like `"hello"`) is stored as bytes. Each letter has a number (e.g. `'h'` = 104, `'e'` = 101). So **`[]byte("hello")`** is just **`[104, 101, 108, 108, 111]`**.
- **String** and **[]byte** both hold text; the difference is the **type**. Many Go functions (e.g. **file.Read**, **json.Marshal**) work with **[]byte**, not **string**. So you often convert: **`string(bytes)`** and **`[]byte(str)`**.

**Simple rule:**  
- If you have **text** and want to **process** it (search, split, replace), you can use **string** and the **strings** package, or **[]byte** and the **bytes** package.  
- If you have **data from a file or network** (which comes as **[]byte**), use the **bytes** package to work on it without converting to string every time.

---

## When to use `bytes` vs `strings`? (Common doubt)

| Use **strings** when | Use **bytes** when |
|----------------------|---------------------|
| You have a **string** (e.g. `s := "hello"`) | You have **[]byte** (e.g. from **ReadFile**, **Read**, or **Marshal**) |
| You only work with text in memory | You read/write files or network and want to avoid converting to string |
| You want **strings.Contains(s, "x")** | You want **bytes.Contains(b, []byte("x"))** |

If your data is already **string**, use **strings**. If it is already **[]byte**, use **bytes**. You can always convert: **`[]byte(s)`** and **`string(b)`**.

---

## Why do I see numbers when I print `[]byte`? (Common doubt)

When you **`fmt.Println(bytes.Split(...))`** you see something like **`[[104 101 108 108 111] ...]`**. Those numbers are the **byte values** (e.g. 104 = `'h'`). Go prints **[]byte** as numbers, not as letters. To see text, convert to string: **`string(byteSlice)`** or **`fmt.Println(string(byteSlice))`**.

---

## Main functions

| Function | What it does |
|----------|----------------|
| **`bytes.Contains(b, sub)`** | true if **b** contains **sub** |
| **`bytes.Split(b, sep)`** | split **b** by **sep** into a slice of **[]byte** |
| **`bytes.Join(slices, sep)`** | join slices with **sep** between them |
| **`bytes.ReplaceAll(b, old, new)`** | replace all **old** with **new** |
| **`bytes.TrimSpace(b)`** | remove spaces (and similar) from start and end |
| **`bytes.Equal(a, b)`** | true if **a** and **b** are the same bytes |

---

## Example

```go
package main

import (
    "bytes"
    "fmt"
)

func main() {
    b := []byte("hello, world")
    fmt.Println(bytes.Contains(b, []byte("world")))  // true
    fmt.Println(bytes.Split(b, []byte(", ")))        // [[104 101 108 108 111] [119 111 114 108 100]]

    parts := [][]byte{[]byte("a"), []byte("b"), []byte("c")}
    joined := bytes.Join(parts, []byte("-"))
    fmt.Println(string(joined))  // a-b-c

    trimmed := bytes.TrimSpace([]byte("  hi  \n"))
    fmt.Println(string(trimmed))  // hi
}
```

- **`[]byte("hello")`** converts a string to a byte slice.
- **`string(byteSlice)`** converts a byte slice back to a string.

---

## Other useful packages (quick list)

| Package | What it does | Full doc |
|---------|----------------|----------|
| **`strings`** | Work with strings | [21-strings.md](21-strings.md) |
| **`strconv`** | Number ↔ string | [22-type-conversion.md](22-type-conversion.md) |
| **`math`** | Math functions | [23-numbers-math.md](23-numbers-math.md) |
| **`slices`** | Contains, Sort, Clone, Delete | [09-arrays-and-slices.md](09-arrays-and-slices.md) |
| **`time`** | Current time, sleep, format dates | [30-time.md](30-time.md) |
| **`regexp`** | Match or replace patterns in text | [32-regexp.md](32-regexp.md) |
| **`encoding/base64`** | Encode/decode base64 (e.g. binary in JSON) | [31-encoding-base64.md](31-encoding-base64.md) |

---

## Summary

| Task | Function |
|------|----------|
| Contains | **`bytes.Contains(b, sub)`** |
| Split / Join | **`bytes.Split`**, **`bytes.Join`** |
| Replace / Trim | **`bytes.ReplaceAll`**, **`bytes.TrimSpace`** |
| Compare | **`bytes.Equal(a, b)`** |

**← [Back to INDEX](INDEX.md)** | Next: [30-time.md](30-time.md) – **Time**: current time, sleep, format. See also: [31-encoding-base64.md](31-encoding-base64.md), [32-regexp.md](32-regexp.md).
