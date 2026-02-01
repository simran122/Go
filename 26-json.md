# JSON in Go (`encoding/json`)

**← [Back to INDEX](INDEX.md)**

JSON is a common format for **APIs** and **config files**. Go can turn **structs** and **maps** into JSON (**marshal**) and turn JSON back into Go values (**unmarshal**). Everything is in simple language.

---

## Turn Go → JSON (Marshal)

**`json.Marshal(value)`** turns a Go value (struct, map, slice) into **JSON bytes**. It returns **`[]byte`** and an error.

```go
package main

import (
    "encoding/json"
    "fmt"
)

type Person struct {
    Name string `json:"name"`
    Age  int    `json:"age"`
}

func main() {
    p := Person{Name: "Alice", Age: 25}
    data, err := json.Marshal(p)
    if err != nil {
        panic(err)
    }
    fmt.Println(string(data))  // {"name":"Alice","age":25}
}
```

- **`json:"name"`** is a **tag**: it tells Go to use **"name"** as the JSON key instead of **"Name"**.
- **`string(data)`** converts the JSON bytes to a string so you can print or write to a file.

---

## Turn JSON → Go (Unmarshal)

**What is `&p`?** **`&p`** is the **address** of **p**. We pass a **pointer** so **Unmarshal** can **write into** **p**. If we passed **p** (not **&p**), the function would get a copy and could not change **p**; with **&p**, it fills **p** with the data from the JSON.

**`json.Unmarshal(jsonBytes, &value)`** fills a Go variable from JSON. You pass a **pointer** (**&value**) so the function can fill it.

```go
package main

import (
    "encoding/json"
    "fmt"
)

type Person struct {
    Name string `json:"name"`
    Age  int    `json:"age"`
}

func main() {
    jsonStr := `{"name":"Bob","age":30}`
    var p Person
    err := json.Unmarshal([]byte(jsonStr), &p)
    if err != nil {
        panic(err)
    }
    fmt.Println(p.Name, p.Age)  // Bob 30
}
```

- **`&p`** – we pass a **pointer** so **Unmarshal** can write into **p**.
- **`[]byte(jsonStr)`** – **Unmarshal** needs **[]byte**, not a string.

---

## Pretty JSON (indented)

**`json.MarshalIndent(value, prefix, indent)`** produces JSON with newlines and indentation (good for config files or logs).

```go
data, err := json.MarshalIndent(p, "", "  ")
fmt.Println(string(data))
// {
//   "name": "Alice",
//   "age": 25
// }
```

---

## Summary

| Task | Function |
|------|----------|
| Go → JSON | **`json.Marshal(value)`** |
| JSON → Go | **`json.Unmarshal(bytes, &value)`** |
| Pretty JSON | **`json.MarshalIndent(value, "", "  ")`** |

**← [Back to INDEX](INDEX.md)** | Next: [27-csv.md](27-csv.md) – **CSV**: read and write CSV files.
