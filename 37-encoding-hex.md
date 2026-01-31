# Hex Encoding in Go (`encoding/hex` package)

**← [Back to INDEX](INDEX.md)**

The **`encoding/hex`** package converts **bytes** to **hexadecimal strings** and back. Useful for hashes, binary data in text, or debugging. Everything is in simple language.

---

## What is hex?

- **Hex** = hexadecimal = base 16. Uses digits **0–9** and letters **A–F** (or **a–f**).
- Each **byte** (0–255) is two hex characters (e.g. **104** → **"68"**, **255** → **"ff"**).
- So **[]byte("Hi")** → **"4869"** (H=72=0x48, i=105=0x69).

---

## Encode (bytes → hex string)

**`hex.EncodeToString(data)`** converts **`[]byte`** into a **hex string**. No error; always valid.

```go
package main

import (
    "encoding/hex"
    "fmt"
)

func main() {
    data := []byte("Hello")
    encoded := hex.EncodeToString(data)
    fmt.Println(encoded)  // 48656c6c6f
}
```

- **`data`** – raw bytes (e.g. text, file content, hash).
- **`encoded`** – string of hex characters (0–9, a–f).

---

## Decode (hex string → bytes)

**`hex.DecodeString(str)`** converts a **hex string** into **`[]byte`**. Returns **`([]byte, error)`**. The string must have **even** length and only valid hex characters.

```go
encoded := "48656c6c6f"
decoded, err := hex.DecodeString(encoded)
if err != nil {
    panic(err)
}
fmt.Println(string(decoded))  // Hello
```

- **`decoded`** – raw bytes. Use **`string(decoded)`** if it is text.

---

## Decode with DecodedLen

**`hex.DecodedLen(n)`** returns how many bytes a hex string of length **n** will produce: **n/2**.

```go
s := "48656c6c6f"
buf := make([]byte, hex.DecodedLen(len(s)))
n, err := hex.Decode(buf, []byte(s))
if err != nil {
    panic(err)
}
fmt.Println(string(buf[:n]))  // Hello
```

- **`hex.Decode(dst, src)`** – decode **src** (hex bytes) into **dst**. Returns number of bytes written and error.

---

## Encode to []byte (not string)

**`hex.Encode(dst, src)`** encodes **src** (bytes) into **dst** (hex bytes). **dst** must be at least **hex.EncodedLen(len(src))** bytes.

```go
src := []byte("Hi")
dst := make([]byte, hex.EncodedLen(len(src)))
hex.Encode(dst, src)
fmt.Println(string(dst))  // 4869
```

- **`hex.EncodedLen(n)`** – number of hex bytes needed for **n** input bytes: **2*n**.

---

## Dump (for debugging)

**`hex.Dump(data)`** returns a **string** that looks like a hex dump: offset, hex bytes, and ASCII. Good for debugging binary data.

```go
data := []byte("Hello, Go!")
fmt.Println(hex.Dump(data))
// 00000000  48 65 6c 6c 6f 2c 20 47  6f 21                    |Hello, Go!|
```

---

## Summary

| Task | Function |
|------|----------|
| Encode bytes → string | **`hex.EncodeToString(data)`** |
| Decode string → bytes | **`hex.DecodeString(str)`** |
| Encode to []byte | **`hex.Encode(dst, src)`**, **`hex.EncodedLen(n)`** |
| Decode to []byte | **`hex.Decode(dst, src)`**, **`hex.DecodedLen(n)`** |
| Debug dump | **`hex.Dump(data)`** |

**← [Back to INDEX](INDEX.md)** | Next: [38-sync.md](38-sync.md) – **sync**: Mutex and WaitGroup.
