# Strings and the `strings` Package in Go

**← [Back to INDEX](INDEX.md)**

In Go, a **string** is a value that holds text. Go does **not** give strings “methods” like in some other languages (e.g. `s.length` or `s.toUpperCase()`). Instead, you use **built-in functions** and the **`strings`** package. This page shows the main ways to work with strings.

---

## Built-in: length and indexing

- **`len(s)`** – number of **bytes** in the string (not always the same as “number of characters” for Unicode).
- **`s[i]`** – the byte at index `i` (use **`s[i]`** only when you know the index is valid).

```go
package main

import "fmt"

func main() {
    s := "hello"
    fmt.Println(len(s))  // 5
    fmt.Println(s[0])    // 104 (byte value of 'h')
    fmt.Println(string(s[0]))  // "h"
}
```

---

## String concatenation (joining)

Use the **`+`** operator to join two strings:

```go
a := "Hello"
b := "World"
c := a + ", " + b  // "Hello, World"
```

For many strings, **`strings.Join`** is better (see below).

---

## The `strings` package – main functions

Import **`"strings"`** and use these **functions** (you pass the string as the first argument).

### Check if a string contains something

| Function | What it does | Example |
|---------|----------------|---------|
| **`strings.Contains(s, substr)`** | true if `s` contains `substr` | `strings.Contains("hello", "ell")` → `true` |
| **`strings.HasPrefix(s, prefix)`** | true if `s` starts with `prefix` | `strings.HasPrefix("hello", "he")` → `true` |
| **`strings.HasSuffix(s, suffix)`** | true if `s` ends with `suffix` | `strings.HasSuffix("hello", "lo")` → `true` |

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    s := "hello world"
    fmt.Println(strings.Contains(s, "world"))   // true
    fmt.Println(strings.HasPrefix(s, "hello"))   // true
    fmt.Println(strings.HasSuffix(s, "world"))   // true
}
```

---

### Find position (index)

| Function | What it does |
|---------|----------------|
| **`strings.Index(s, substr)`** | Index of first occurrence of `substr` in `s`, or `-1` |
| **`strings.LastIndex(s, substr)`** | Index of last occurrence, or `-1` |
| **`strings.IndexAny(s, chars)`** | Index of first character from `chars` that appears in `s` |

```go
s := "hello"
fmt.Println(strings.Index(s, "l"))   // 2
fmt.Println(strings.Index(s, "x"))   // -1
fmt.Println(strings.LastIndex(s, "l"))  // 3
```

---

### Replace and change case

| Function | What it does |
|---------|----------------|
| **`strings.Replace(s, old, new, n)`** | Replace `old` with `new` in `s`. `n` = how many times (-1 = all) |
| **`strings.ReplaceAll(s, old, new)`** | Replace **all** occurrences |
| **`strings.ToUpper(s)`** | All letters uppercase |
| **`strings.ToLower(s)`** | All letters lowercase |
| **`strings.ToTitle(s)`** | Title case (each word’s first letter uppercase) |

```go
s := "Hello World"
fmt.Println(strings.Replace(s, "World", "Go", 1))  // Hello Go
fmt.Println(strings.ToLower(s))   // hello world
fmt.Println(strings.ToUpper(s))   // HELLO WORLD
```

---

### Split and join

| Function | What it does |
|---------|----------------|
| **`strings.Split(s, sep)`** | Split `s` by `sep` into a slice of strings |
| **`strings.Join(slice, sep)`** | Join slice of strings with `sep` between them |

```go
s := "a,b,c"
parts := strings.Split(s, ",")     // []string{"a", "b", "c"}
joined := strings.Join(parts, "-") // "a-b-c"
fmt.Println(parts, joined)
```

---

### Trim (remove from start/end)

| Function | What it does |
|---------|----------------|
| **`strings.TrimSpace(s)`** | Remove spaces (and similar) from start and end |
| **`strings.Trim(s, cutset)`** | Remove characters in `cutset` from start and end |
| **`strings.TrimPrefix(s, prefix)`** | Remove `prefix` only from start |
| **`strings.TrimSuffix(s, suffix)`** | Remove `suffix` only from end |

```go
s := "  hello  "
fmt.Println(strings.TrimSpace(s))  // "hello"
fmt.Println(strings.Trim("!!hi!!", "!"))  // "hi"
fmt.Println(strings.TrimPrefix("hello world", "hello "))  // "world"
```

---

### Count and repeat

| Function | What it does |
|---------|----------------|
| **`strings.Count(s, substr)`** | How many times `substr` appears in `s` (non-overlapping) |
| **`strings.Repeat(s, n)`** | String `s` repeated `n` times |

```go
fmt.Println(strings.Count("cheese", "e"))  // 3
fmt.Println(strings.Repeat("ab", 3))       // "ababab"
```

---

### Compare

| Function | What it does |
|---------|----------------|
| **`strings.Compare(a, b)`** | Compare two strings: `0` if equal, `-1` if a < b, `1` if a > b |
| **`strings.EqualFold(a, b)`** | Case-**insensitive** equality |

```go
fmt.Println(strings.Compare("a", "b"))   // -1
fmt.Println(strings.EqualFold("Go", "go"))  // true
```

---

## Summary: strings in Go

- **No “string methods”** – use the **`strings`** package and pass the string as the first argument.
- **Length:** **`len(s)`** (bytes).
- **Join:** **`+`** or **`strings.Join`**.
- **Contains / prefix / suffix:** **`strings.Contains`**, **`strings.HasPrefix`**, **`strings.HasSuffix`**.
- **Replace / case:** **`strings.Replace`**, **`strings.ToUpper`**, **`strings.ToLower`**.
- **Split / join:** **`strings.Split`**, **`strings.Join`**.
- **Trim:** **`strings.TrimSpace`**, **`strings.Trim`**, **`strings.TrimPrefix`**, **`strings.TrimSuffix`**.

**← [Back to INDEX](INDEX.md)** | Next: [22-type-conversion.md](22-type-conversion.md) – **Type conversion**: converting between numbers, strings, and bytes.
