# HTTP in Go (`net/http` package)

**← [Back to INDEX](INDEX.md)**

The **`net/http`** package lets you build an **HTTP server** (e.g. a website or API) and an **HTTP client** (e.g. call other websites). Everything is in simple language.

**Concepts used in this page:** We use **defer** ([19-defer-panic-recover.md](19-defer-panic-recover.md)) to close response bodies. Read that first if you haven’t.

---

## Why use net/http?

- **Server** – handle HTTP requests (GET, POST, etc.) and send responses (HTML, JSON, etc.).
- **Client** – send HTTP requests (GET, POST) to other URLs and read the response.

---

## Simple HTTP server

**What is `w` and `r`?**  
When someone visits your site, Go calls your **handler** with two arguments: **`w`** and **`r`**.  
- **`w`** = **where to write the response**. Same idea as the **w** in **`fmt.Fprintf(w, ...)`** – whatever you write to **w** is sent back to the user’s browser. Type: **`http.ResponseWriter`**.  
- **`r`** = **the request** – what the user asked for: the URL path, method (GET, POST), query params, headers, body, etc. Type: **`*http.Request`**.

**`http.HandleFunc(path, handler)`** – when someone visits **path**, run **handler**.  
**`http.ListenAndServe(addr, nil)`** – start the server on **addr** (e.g. **":8080"**).

```go
package main

import (
    "fmt"
    "net/http"
)

func main() {
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, "Hello, world!")  // w = where the response goes (browser)
    })
    http.HandleFunc("/hello", func(w http.ResponseWriter, r *http.Request) {
        name := r.URL.Query().Get("name")  // r = the request (e.g. ?name=Alice)
        if name == "" {
            name = "Guest"
        }
        fmt.Fprintf(w, "Hello, %s!", name)
    })
    http.ListenAndServe(":8080", nil)
}
```

- **`w`** – write the **response** here: **`fmt.Fprintf(w, "Hello")`** or **`w.Write([]byte("Hi"))`**.
- **`r`** – the **request**: **`r.URL.Path`** (path), **`r.Method`** (GET, POST), **`r.URL.Query()`** (query params), **`r.Header`** (headers).
- **`:8080`** – listen on port **8080** on all interfaces.

**Run:** open **http://localhost:8080** and **http://localhost:8080/hello?name=Alice** in a browser.

---

## Request: path, method, query, headers

| Field / method | What it is |
|----------------|------------|
| **`r.URL.Path`** | Path (e.g. **"/hello"**) |
| **`r.Method`** | HTTP method (e.g. **"GET"**, **"POST"**) |
| **`r.URL.Query()`** | Query params (e.g. **?name=Alice**); **`q.Get("name")`** |
| **`r.Header.Get("Content-Type")`** | Request header |
| **`r.Body`** | Request body (where the data comes from); use **io.ReadAll(r.Body)** to read it all |

---

## Response: status, headers, body

| Method | What it does |
|--------|----------------|
| **`w.WriteHeader(statusCode)`** | Set status (e.g. **200**, **404**, **500**). Call **before** writing body. |
| **`w.Header().Set("Key", "Value")`** | Set response header |
| **`w.Write(data)`** | Write bytes to the response body |
| **`fmt.Fprintf(w, ...)`** | Write formatted string to body |

```go
w.Header().Set("Content-Type", "application/json")
w.WriteHeader(http.StatusOK)
fmt.Fprintf(w, `{"ok": true}`)
```

---

## Serve files (static files)

**`http.FileServer(root)`** serves files from a directory. **`http.StripPrefix(prefix, handler)`** removes a prefix from the URL path.

```go
fs := http.FileServer(http.Dir("./static"))
http.Handle("/static/", http.StripPrefix("/static", fs))
```

So **/static/style.css** serves **./static/style.css**.

---

## HTTP client: GET a URL

**What is `resp`?** **`resp`** is the **response** from the server – status code, headers, and body. Type **`*http.Response`**.

**What is `resp.Body`?** **`resp.Body`** is **where the response data comes from** – you read it like a file or a Reader. Use **`io.ReadAll(resp.Body)`** to read everything into **[]byte**. **Always** call **`defer resp.Body.Close()`** when you are done so the connection can be reused.

**`http.Get(url)`** sends a **GET** request and returns the response and an error.

```go
resp, err := http.Get("https://example.com")
if err != nil {
    log.Fatal(err)
}
defer resp.Body.Close()

body, err := io.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Println(string(body))
```

- **`resp.StatusCode`** – status (e.g. **200**, **404**).
- **`resp.Body`** – where the response data comes from; read with **io.ReadAll(resp.Body)**. **Always** **defer resp.Body.Close()**.
- **`io.ReadAll(resp.Body)`** – read entire body into **[]byte**.

---

## HTTP client: POST with body

**`http.Post(url, contentType, body)`** sends a **POST** request. **Body** is **io.Reader** (e.g. **strings.NewReader(jsonStr)**).

```go
jsonBody := `{"name": "Alice"}`
resp, err := http.Post("https://example.com/api", "application/json", strings.NewReader(jsonBody))
if err != nil {
    log.Fatal(err)
}
defer resp.Body.Close()
```

---

## HTTP client: custom request (with context, headers)

**`http.NewRequestWithContext(ctx, method, url, body)`** creates a request. **`http.DefaultClient.Do(req)`** sends it.

```go
ctx := context.Background()
req, err := http.NewRequestWithContext(ctx, "GET", "https://example.com", nil)
if err != nil {
    log.Fatal(err)
}
req.Header.Set("Authorization", "Bearer token123")
resp, err := http.DefaultClient.Do(req)
if err != nil {
    log.Fatal(err)
}
defer resp.Body.Close()
```

- Use **context** for **timeout** or **cancellation** (see [41-context.md](41-context.md)).

---

## Summary

| Task | Function / idea |
|------|------------------|
| Register handler | **`http.HandleFunc("/", handler)`** |
| Start server | **`http.ListenAndServe(":8080", nil)`** |
| Request | **`r.URL.Path`**, **`r.Method`**, **`r.URL.Query()`**, **`r.Body`** |
| Response | **`w.WriteHeader`**, **`w.Header().Set`**, **`w.Write`**, **`fmt.Fprintf(w, ...)`** |
| GET | **`http.Get(url)`** |
| POST | **`http.Post(url, contentType, body)`** |
| Custom request | **`http.NewRequestWithContext`** + **`Client.Do(req)`** |

**← [Back to INDEX](INDEX.md)** | Next: [36-net-url.md](36-net-url.md) – **net/url**: parse URLs and query params.
