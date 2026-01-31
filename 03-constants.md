# Constants in Go

**← [Back to INDEX](INDEX.md)**

A **constant** is a value that **does not change** while the program runs. Use constants for things like fixed numbers, app names, or settings.

---

## Why use constants?

- The value is **fixed** – no one can change it by mistake.
- The name makes the code **clearer** (e.g. `MaxUsers` instead of `100`).
- You can change the value in **one place** and it updates everywhere.

---

## Simple constants with `const`

```go
package main

import "fmt"

func main() {
    const Pi = 3.14159
    const AppName = "MyApp"
    fmt.Println(Pi, AppName)  // 3.14159 MyApp
}
```

**Pattern:** `const name = value`

You **cannot** do: `Pi = 3.14` later – that would be an error, because constants cannot be changed.

---

## Multiple constants in a block

You can group constants inside `const ( ... )`.

```go
package main

import "fmt"

func main() {
    const (
        Monday    = 1
        Tuesday   = 2
        Wednesday = 3
    )
    fmt.Println(Monday, Tuesday, Wednesday)  // 1 2 3
}
```

---

## Using `iota` for sequential numbers

**`iota`** is a special name in Go. In a `const` block it starts at 0 and increases by 1 for each line.

```go
package main

import "fmt"

func main() {
    const (
        Sunday = iota  // 0
        Monday         // 1
        Tuesday        // 2
        Wednesday      // 3
    )
    fmt.Println(Sunday, Monday, Tuesday, Wednesday)  // 0 1 2 3
}
```

If you don’t write a value, the next line **repeats the same expression** with the next `iota`. So `Monday` gets `iota` (1), `Tuesday` gets 2, and so on.

---

## `iota` with expressions

You can use `iota` in a formula. This example uses bit-shifting to define sizes:

```go
package main

import "fmt"

func main() {
    const (
        KB = 1 << (10 * iota)  // 1 << 0 = 1
        MB                      // 1 << 10
        GB                      // 1 << 20
    )
    fmt.Println(KB, MB, GB)
}
```

You don’t have to understand bit-shifting yet – just remember that **`iota`** gives 0, 1, 2, … in order inside a `const` block.

---

## Summary

| Idea | Example |
|------|---------|
| Single constant | `const Pi = 3.14159` |
| Block of constants | `const ( A = 1; B = 2 )` |
| Sequential values | `const ( Zero = iota; One; Two )` |

**Rule:** Constants are **fixed** – you cannot change them after they are defined.

**← [Back to INDEX](INDEX.md)** | Next: [04-basic-types.md](04-basic-types.md) – **Basic types**: numbers, text, and true/false.
