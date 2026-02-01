# Base64 in Go (`encoding/base64`)

**← [Back to INDEX](INDEX.md)**

**Base64** turns **binary data** (or any bytes) into a **text string** that is safe to use in JSON, URLs, or emails. The **`encoding/base64`** package **encodes** (bytes → string) and **decodes** (string → bytes). Everything is in simple language.

---

## What is Base64? (Simple idea)

- **Normal text** uses letters and numbers (A–Z, a–z, 0–9, etc.).  
- **Binary data** (e.g. a file or an image) is just bytes (numbers 0–255). You cannot put raw bytes directly in JSON or a URL.  
- **Base64** converts those bytes into a **string** that uses only safe characters (A–Z, a–z, 0–9, +, /). So you can store or send binary data as text.

**When to use:** When you need to put **binary data** inside **JSON**, a **URL**, or an **email**, or when you read/write encoded data from an API.

---

## Encode (bytes → Base64 string)

**`base64.StdEncoding.EncodeToString(data)`** converts **`[]byte`** into a **Base64 string**.

```go
package main

import (
    "encoding/base64"
    "fmt"
)

func main() {
    data := []byte("Hello, Go!")
    encoded := base64.StdEncoding.EncodeToString(data)
    fmt.Println(encoded)  // SGVsbG8sIEdvIQ==
}
```

- **`data`** is **`[]byte`** – the raw bytes (e.g. text or file content).  
- **`encoded`** is a **string** – safe to put in JSON or a URL.

---

## Decode (Base64 string → bytes)

**`base64.StdEncoding.DecodeString(str)`** converts a **Base64 string** back into **`[]byte`**. It returns **`([]byte, error)`** – always check the error.

```go
encoded := "SGVsbG8sIEdvIQ=="
decoded, err := base64.StdEncoding.DecodeString(encoded)
if err != nil {
    panic(err)
}
fmt.Println(string(decoded))  // Hello, Go!
```

- **`decoded`** is **`[]byte`**. Use **`string(decoded)`** to get a string if it is text.

---

## Example: use in JSON

Sometimes APIs send binary data (e.g. an image) as a Base64 string inside JSON. You decode the string to get the bytes.

```go
// Suppose you get this from JSON: "imageData": "SGVsbG8sIEdvIQ=="
imageBase64 := "SGVsbG8sIEdvIQ=="
imageBytes, err := base64.StdEncoding.DecodeString(imageBase64)
if err != nil {
    panic(err)
}
// Now imageBytes is []byte – you can save to a file or process it.
```

---

## Summary

| Task | Function |
|------|----------|
| Encode (bytes → string) | **`base64.StdEncoding.EncodeToString(data)`** |
| Decode (string → bytes) | **`base64.StdEncoding.DecodeString(str)`** |

**← [Back to INDEX](INDEX.md)** | Next: [32-regexp.md](32-regexp.md) – **Regexp**: match or replace patterns in text. See also: [29-bytes.md](29-bytes.md) – **Bytes**.
