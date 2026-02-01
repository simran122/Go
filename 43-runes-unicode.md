# Runes and Unicode in Go

**← [Back to INDEX](INDEX.md)**

In Go, a **string** is a sequence of **bytes**. To work with **characters** (especially beyond plain ASCII), you use **runes**. A **rune** is a single Unicode **code point** – one character – and in Go it is an alias for **`int32`**. This page explains runes, UTF-8, and how to use them in strings.

---

## Why runes?

- **ASCII** uses one byte per character (e.g. `'A'` = 65). Many characters in the world (é, 中, 🎉) need **multiple bytes** in **UTF-8**.
- In Go, **`range`** over a string gives you **runes** (code points), not raw bytes. So you can iterate "character by character" correctly.
- The type **`rune`** is just **`int32`**; it holds one Unicode code point.

---

## Rune literal

Use **single quotes** for a **single rune** (one character).

```go
var r rune = 'A'       // same as int32(65)
var heart rune = '❤'   // Unicode heart
fmt.Println(r, heart)  // 65 10084
```

**Single quotes** = one rune. **Double quotes** = string (even if one character: **`"A"`** is a **`string`**, **`'A'`** is a **`rune`**).

---

## String is UTF-8

Go source code is UTF-8, and Go **strings** are conventionally **UTF-8** encoded. So a string like **`"café"`** or **`"Hello, 世界"`** is stored as UTF-8 bytes. **`len(s)`** returns the **number of bytes**, not the number of runes.

```go
s := "café"
fmt.Println(len(s))  // 5 (é is 2 bytes in UTF-8)

s2 := "世界"
fmt.Println(len(s2))  // 6 (two 3-byte characters)
```

---

## Range over string gives runes

When you **`range`** over a string, you get **index** and **rune** (code point). So you can count or process **characters** correctly.

```go
s := "Hello, 世界"
for i, r := range s {
    fmt.Printf("%d: %c (U+%04X)\n", i, r, r)
}
// 0: H (U+0048)
// 1: e (U+0065)
// ... 
// 7: 世 (U+4E16)
// 10: 界 (U+754C)
```

**`%c`** prints the character; **`U+%04X`** prints the Unicode code point.

---

## Rune count (number of characters)

Use **`utf8.RuneCountInString(s)`** from **`unicode/utf8`** to get the number of **runes** (characters), not bytes.

```go
package main

import (
    "fmt"
    "unicode/utf8"
)

func main() {
    s := "café"
    fmt.Println(len(s))                    // 5 (bytes)
    fmt.Println(utf8.RuneCountInString(s)) // 4 (runes)
}
```

---

## Converting rune ↔ string

- **Rune to string:** **`string(r)`** – one rune becomes a string (1–4 bytes in UTF-8).
- **String to runes:** **`[]rune(s)`** – slice of runes (one element per character). **`string([]rune(s))`** converts back to string.

```go
r := '世'
s := string(r)       // "世"
runes := []rune("世界")  // []rune{'世', '界'}
fmt.Println(string(runes))  // "世界"
```

---

## Useful packages

- **`unicode/utf8`** – **RuneCountInString**, **DecodeRuneInString**, **ValidString**, etc.
- **`unicode`** – **IsLetter**, **IsDigit**, **IsSpace**, **ToUpper**, **ToLower** (work on **rune**).

```go
import "unicode"

if unicode.IsLetter(r) { ... }
if unicode.IsDigit(r) { ... }
upper := unicode.ToUpper(r)
```

---

## Summary

| Idea | Example |
|------|---------|
| Rune | One Unicode code point; type **`rune`** = **`int32`** |
| Rune literal | **`'A'`**, **`'世'`** (single quotes) |
| String = bytes | **`len(s)`** = byte count; use **utf8.RuneCountInString(s)** for character count |
| Range over string | **`for i, r := range s`** – **r** is rune |
| Rune ↔ string | **`string(r)`**, **`[]rune(s)`**, **`string([]rune(s))`** |
| Unicode helpers | **unicode/utf8**, **unicode** (IsLetter, IsDigit, ToUpper, etc.) |

**← [Back to INDEX](INDEX.md)** | Prev: [42-closures.md](42-closures.md) | Next: [44-new-vs-make.md](44-new-vs-make.md) – **new vs make**.
