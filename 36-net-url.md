# URL Parsing in Go (`net/url` package)

**← [Back to INDEX](INDEX.md)**

The **`net/url`** package lets you **parse** URLs into parts (scheme, host, path, query) and **build** or **encode** URLs. Everything is in simple language.

**What is `u`?** **`u`** is the **parsed URL** – the parts of the URL (scheme, host, path, query). Type **`*url.URL`**. You get it from **`url.Parse(str)`**.

**What is `q`?** **`q`** is the **query parameters** – the **?key=value** part of the URL. You get it from **`u.Query()`**. Use **`q.Get("key")`** to get the first value for a key.

---

## Why use net/url?

- **Parse** a URL string into **scheme**, **host**, **path**, **query**.
- **Read query parameters** (e.g. **?name=Alice&age=25**).
- **Build** or **encode** URLs safely (e.g. for query params with spaces or special characters).

---

## Parse a URL

**`url.Parse(str)`** parses a URL string and returns **`(*url.URL, error)`**. Always check the error.

```go
package main

import (
    "fmt"
    "net/url"
)

func main() {
    u, err := url.Parse("https://example.com/path?name=Alice&age=25")
    if err != nil {
        panic(err)
    }
    fmt.Println(u.Scheme)    // https
    fmt.Println(u.Host)      // example.com
    fmt.Println(u.Path)     // /path
    fmt.Println(u.RawQuery)  // name=Alice&age=25
}
```

---

## URL parts

| Field | What it is |
|-------|------------|
| **`u.Scheme`** | Protocol (e.g. **"https"**, **"http"**) |
| **`u.Host`** | Host (e.g. **"example.com"**, **"example.com:8080"**) |
| **`u.Path`** | Path (e.g. **"/path"**, **"/api/users"**) |
| **`u.RawQuery`** | Query string as-is (e.g. **"name=Alice&age=25"**) |
| **`u.Fragment`** | Fragment (e.g. **"section1"** from **#section1**) |
| **`u.User`** | User info (e.g. **user:pass** in **https://user:pass@host**) |

---

## Query parameters

**`u.Query()`** returns **url.Values** (a map of **key → []string**). Use **`q.Get("key")`** to get the **first** value for a key.

```go
u, _ := url.Parse("https://example.com?name=Alice&age=25&tag=go&tag=lang")
q := u.Query()
fmt.Println(q.Get("name"))  // Alice
fmt.Println(q.Get("age"))  // 25
fmt.Println(q.Get("tag"))  // go (first value only)
fmt.Println(q["tag"])      // [go lang] (all values)
```

- **`q.Get("name")`** – first value for **"name"**; **""** if missing.
- **`q["tag"]`** – slice of all values for **"tag"**.

---

## Build a URL (with query params)

**`url.Values`** is a map: **`q.Set("key", "value")`**, **`q.Add("key", "value")`**. Then **`q.Encode()`** turns it into a query string (e.g. **"name=Alice&age=25"**).

```go
u, _ := url.Parse("https://example.com/api")
q := u.Query()
q.Set("name", "Alice")
q.Set("age", "25")
u.RawQuery = q.Encode()
fmt.Println(u.String())  // https://example.com/api?age=25&name=Alice
```

- **`q.Set("key", "value")`** – set one value (replaces existing).
- **`q.Add("key", "value")`** – add another value for the same key.
- **`q.Encode()`** – encode for use in URL (spaces → **+** or **%20**, etc.).

---

## Encode and decode (path or query)

**`url.PathEscape(s)`** – encode a string for use in a **path** (e.g. **/file/hello%20world**).  
**`url.PathUnescape(s)`** – decode a path-encoded string.  
**`url.QueryEscape(s)`** – encode for **query** (e.g. **name=hello%20world**).  
**`url.QueryUnescape(s)`** – decode a query-encoded string.

```go
encoded := url.QueryEscape("hello world")
fmt.Println(encoded)  // hello+world or hello%20world

decoded, err := url.QueryUnescape(encoded)
if err != nil {
    panic(err)
}
fmt.Println(decoded)  // hello world
```

---

## Build a URL from parts

**`url.URL`** is a struct. You can set **Scheme**, **Host**, **Path**, **RawQuery** and then call **`u.String()`** to get the full URL.

```go
u := &url.URL{
    Scheme: "https",
    Host:   "example.com",
    Path:   "/search",
    RawQuery: "q=hello",
}
fmt.Println(u.String())  // https://example.com/search?q=hello
```

---

## Summary

| Task | Function / idea |
|------|------------------|
| Parse URL | **`url.Parse(str)`** |
| Parts | **`u.Scheme`**, **`u.Host`**, **`u.Path`**, **`u.RawQuery`** |
| Query params | **`u.Query()`** → **`q.Get("key")`**, **`q["key"]`** |
| Build query | **`q.Set`**, **`q.Add`**, **`q.Encode()`**, **`u.RawQuery = q.Encode()`** |
| Encode/decode | **`url.QueryEscape`**, **`url.QueryUnescape`**, **`url.PathEscape`**, **`url.PathUnescape`** |
| Full URL | **`u.String()`** |

**← [Back to INDEX](INDEX.md)** | Next: [37-encoding-hex.md](37-encoding-hex.md) – **encoding/hex**: hex encode and decode.
