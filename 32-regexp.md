# Regular Expressions in Go (`regexp` package)

**← [Back to INDEX](INDEX.md)**

**Regular expressions** (regex) let you **search** for **patterns** in text (e.g. “all numbers”, “all email-like strings”). The **`regexp`** package compiles a pattern and then you can **match**, **find**, or **replace**. Everything is in simple language.

---

## What is a regex? (Simple idea)

A **regex** is a **pattern** written in a special language. For example:

- **`[0-9]+`** means “one or more digits” (e.g. 42, 100).  
- **`hello`** means the exact text “hello”.  
- **`a.b`** means “a”, then any character, then “b” (e.g. axb, a b).

You give this pattern to **regexp**; it finds where the pattern appears in your string.

---

## Match (does the whole string fit the pattern?)

**`regexp.MatchString(pattern, s)`** returns **true** if the string **s** contains a substring that matches the pattern. It returns **`(bool, error)`** – check the error.

```go
package main

import (
    "fmt"
    "regexp"
)

func main() {
    ok, err := regexp.MatchString(`hello`, "hello world")
    if err != nil {
        panic(err)
    }
    fmt.Println(ok)  // true – "hello" appears

    ok, _ = regexp.MatchString(`[0-9]+`, "abc123")
    fmt.Println(ok)  // true – there are digits
}
```

- Use **backticks** **`` ` ``** for the pattern string so you don’t have to escape backslashes: **`` `[0-9]+` ``**.

---

## Find (get the first match)

**Compile** the pattern once with **`regexp.Compile(pattern)`**, then use **`re.FindString(s)`** to get the **first** substring that matches.

```go
re, err := regexp.Compile(`[0-9]+`)
if err != nil {
    panic(err)
}
s := "I have 2 apples and 5 oranges"
first := re.FindString(s)
fmt.Println(first)  // 2 – first group of digits
```

- **`FindString(s)`** returns one string (the first match), or **""** if nothing matches.

---

## Find all (get every match)

**`re.FindAllString(s, n)`** returns a **slice** of all matches. **n** = how many to return (**-1** = all).

```go
re, _ := regexp.Compile(`[0-9]+`)
s := "I have 2 apples and 5 oranges"
all := re.FindAllString(s, -1)
fmt.Println(all)  // [2 5]
```

---

## Replace

**`re.ReplaceAllString(s, replacement)`** replaces **every** match in **s** with **replacement**.

```go
re, _ := regexp.Compile(`[0-9]+`)
s := "I have 2 apples and 5 oranges"
newStr := re.ReplaceAllString(s, "X")
fmt.Println(newStr)  // I have X apples and X oranges
```

---

## Simple patterns (cheat sheet)

| Pattern | Meaning | Example match |
|---------|---------|----------------|
| **`abc`** | Exact text "abc" | abc |
| **`[0-9]`** | One digit | 5 |
| **`[0-9]+`** | One or more digits | 42, 100 |
| **`[a-z]+`** | One or more lowercase letters | hello |
| **`.`** | Any single character | a, 1, space |
| **`.*`** | Any characters (greedy) | anything |
| **`\s`** | Whitespace (space, tab) | space |
| **`^`** | Start of string | — |
| **`$`** | End of string | — |

Use **backticks** for the pattern: **`` `[0-9]+` ``**.

---

## Summary

| Task | Function |
|------|----------|
| Match (pattern in string?) | **`regexp.MatchString(pattern, s)`** |
| Compile once | **`regexp.Compile(pattern)`** |
| Find first | **`re.FindString(s)`** |
| Find all | **`re.FindAllString(s, -1)`** |
| Replace all | **`re.ReplaceAllString(s, replacement)`** |

**← [Back to INDEX](INDEX.md)** | Next: [33-more-useful-packages.md](33-more-useful-packages.md) – **More useful packages**: flag, log, net/http, url, hex, sync, fmt, io, context, testing. See also: [21-strings.md](21-strings.md) – **Strings** | [29-bytes.md](29-bytes.md) – **Bytes**
