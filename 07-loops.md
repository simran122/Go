# Loops in Go

**← [Back to INDEX](INDEX.md)**

A **loop** runs the same block of code **again and again**. In Go there is mainly one loop keyword: **`for`**. You use it for classic loops and for walking over slices, maps, and strings with **`range`**.

---

## The `for` loop (classic style)

You give a **start**, a **condition** (when to stop), and how to **update** the variable each time.

```go
package main

import "fmt"

func main() {
    for i := 0; i < 5; i++ {
        fmt.Println(i)  // prints 0, 1, 2, 3, 4
    }
}
```

**Meaning:**

- **`i := 0`** – start with `i` equal to 0
- **`i < 5`** – keep looping while `i` is less than 5
- **`i++`** – after each loop, add 1 to `i`

---

## `for` as a “while” loop

If you leave out the start and update, you get a **“while”** style loop: “keep going while the condition is true.”

```go
package main

import "fmt"

func main() {
    count := 0
    for count < 3 {
        fmt.Println(count)
        count++
    }
}
```

---

## Infinite loop

A `for` with **no condition** runs forever (until you break out or exit the program).

```go
for {
    // runs forever unless we use break or return
}
```

You usually use **`break`** inside to exit when something happens.

---

## `for` with `range` (over a slice)

**`range`** gives you each **index** and **value** (or just one of them) when looping over a slice, string, or map.

**Over a slice (index and value):**

```go
package main

import "fmt"

func main() {
    fruits := []string{"apple", "banana", "cherry"}
    for index, value := range fruits {
        fmt.Println(index, value)
    }
}
```

Output:

```
0 apple
1 banana
2 cherry
```

---

## Ignoring the index or value with `_`

If you don’t need the index, use **`_`** so you don’t have an unused variable:

```go
for _, value := range fruits {
    fmt.Println(value)  // only the value
}
```

If you only need the index:

```go
for index := range fruits {
    fmt.Println(index)  // 0, 1, 2
}
```

---

## `range` over a map

Over a **map**, `range` gives you **key** and **value** for each entry.

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

---

## `break` and `continue`

- **`break`** – exit the loop immediately.
- **`continue`** – skip the rest of this iteration and go to the next one.

```go
package main

import "fmt"

func main() {
    for i := 0; i < 10; i++ {
        if i == 3 {
            continue  // skip 3
        }
        if i == 7 {
            break  // stop at 7
        }
        fmt.Println(i)  // 0, 1, 2, 4, 5, 6
    }
}
```

---

## Summary

| Loop form | Use |
|-----------|-----|
| `for i := 0; i < n; i++ { }` | Classic counted loop |
| `for condition { }` | “While” style loop |
| `for { }` | Infinite loop (use `break` to stop) |
| `for i, v := range slice { }` | Loop over slice (index and value) |
| `for k, v := range map { }` | Loop over map (key and value) |

**← [Back to INDEX](INDEX.md)** | Next: [08-functions.md](08-functions.md) – **Functions**: reusable blocks of code and return values.
