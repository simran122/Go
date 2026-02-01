# Maps in Go

**← [Back to INDEX](INDEX.md)**

A **map** stores **key–value pairs**: you look up a **key** and get its **value**. Like a dictionary or a phone book (name → number).

---

## Why use a map?

When you need to **associate** one thing with another:  
user name → age, word → meaning, id → record, etc. Maps give fast lookups by key.

---

## Create and use a map

**Literal (with values):**

```go
package main

import "fmt"

func main() {
    ages := map[string]int{
        "Alice": 25,
        "Bob":   30,
        "Carol": 28,
    }
    fmt.Println(ages["Alice"])  // 25
}
```

**With `make` (empty map):**

```go
scores := make(map[string]int)
scores["math"] = 95
scores["english"] = 88
fmt.Println(scores["math"])  // 95
```

---

## Add and change values

Use **`map[key] = value`** to add a new pair or change an existing one.

```go
ages := map[string]int{"Alice": 25}
ages["Bob"] = 30    // add
ages["Alice"] = 26  // update
fmt.Println(ages)   // map[Alice:26 Bob:30]
```

---

## Check if a key exists

When you read **`m[key]`**, you get the **value** and (optionally) a **bool** that says whether the key was in the map.

```go
package main

import "fmt"

func main() {
    ages := map[string]int{"Alice": 25, "Bob": 30}
    value, ok := ages["Alice"]
    if ok {
        fmt.Println("Alice's age:", value)  // Alice's age: 25
    }
    value, ok = ages["Charlie"]
    if !ok {
        fmt.Println("Charlie not in map")   // Charlie not in map
    }
}
```

If the key is **not** in the map, you get the **zero value** for the value type (e.g. `0` for `int`). So use the **`ok`** flag to tell “found” from “not found”.

---

## Delete a key

Use **`delete(map, key)`**.

```go
ages := map[string]int{"Alice": 25, "Bob": 30}
delete(ages, "Bob")
fmt.Println(ages)  // map[Alice:25]
```

---

## Loop over a map with `range`

**`for key, value := range map`** gives you each key and its value (order is not fixed).

```go
package main

import "fmt"

func main() {
    ages := map[string]int{"Alice": 25, "Bob": 30}
    for name, age := range ages {
        fmt.Println(name, age)
    }
}
```

If you only need keys: **`for key := range ages { }`**.

---

## Nil map

A map that is not initialized is **`nil`**. You **cannot** add to it; you must use **`make`** first.

```go
var m map[string]int  // nil
// m["a"] = 1          // panic!
m = make(map[string]int)
m["a"] = 1             // OK
```

---

## Summary

| Operation | Code |
|-----------|------|
| Create (literal) | `m := map[string]int{"a": 1}` |
| Create (empty) | `m := make(map[string]int)` |
| Get value | `v := m["key"]` |
| Check key | `v, ok := m["key"]` |
| Set/update | `m["key"] = value` |
| Delete | `delete(m, "key")` |
| Loop | `for k, v := range m { }` |

**← [Back to INDEX](INDEX.md)** | Next: [11-structs.md](11-structs.md) – **Structs**: group related data into one type.
