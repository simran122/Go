# Numbers and the `math` Package in Go

**← [Back to INDEX](INDEX.md)**

In Go, **numbers** (int, float64, etc.) do **not** have “methods” like in some other languages. You use **operators** (e.g. `+`, `-`, `*`, `/`) and **functions** from the **`math`** package for things like square root, power, rounding, and min/max.

---

## Built-in: `min` and `max` (Go 1.21+)

You can get the **smallest** or **largest** of two or more values with **`min(...)`** and **`max(...)`**. They work with numbers and strings.

```go
package main

import "fmt"

func main() {
    a, b := 10, 20
    fmt.Println(min(a, b))   // 10
    fmt.Println(max(a, b))   // 20
    fmt.Println(max(5, 10, 3))  // 10
}
```

---

## The `math` package – common functions

Import **`"math"`**. These functions work mainly with **float64**. If you have an **int**, convert first: **`math.Sqrt(float64(n))`**.

### Basic math

| Function | What it does | Example |
|----------|----------------|---------|
| **`math.Abs(x)`** | Absolute value | `math.Abs(-5)` → `5` |
| **`math.Sqrt(x)`** | Square root | `math.Sqrt(16)` → `4` |
| **`math.Pow(x, y)`** | x to the power of y | `math.Pow(2, 3)` → `8` |
| **`math.Cbrt(x)`** | Cube root | `math.Cbrt(27)` → `3` |

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    fmt.Println(math.Abs(-5))      // 5
    fmt.Println(math.Sqrt(16))     // 4
    fmt.Println(math.Pow(2, 3))    // 8
}
```

---

### Rounding

| Function | What it does |
|----------|----------------|
| **`math.Floor(x)`** | Largest integer ≤ x (round down) |
| **`math.Ceil(x)`** | Smallest integer ≥ x (round up) |
| **`math.Round(x)`** | Round to nearest integer |
| **`math.Trunc(x)`** | Drop decimal part (same as int(x) for positive) |

```go
fmt.Println(math.Floor(3.7))  // 3
fmt.Println(math.Ceil(3.2))   // 4
fmt.Println(math.Round(3.5))  // 4
fmt.Println(math.Trunc(3.7)) // 3
```

---

### Min and max (float64)

**`math.Min(x, y)`** and **`math.Max(x, y)`** – for **two** float64 values. For more than two or for ints, use the built-in **`min`** / **`max`** (Go 1.21+).

```go
fmt.Println(math.Min(3.5, 5.2))  // 3.5
fmt.Println(math.Max(3.5, 5.2))  // 5.2
```

---

### Other useful functions

| Function | What it does |
|----------|----------------|
| **`math.Mod(x, y)`** | Remainder of x / y (float) |
| **`math.Sin(x)`**, **`math.Cos(x)`**, **`math.Tan(x)`** | Trigonometry (x in radians) |
| **`math.Exp(x)`** | e^x |
| **`math.Log(x)`** | Natural log |
| **`math.Log10(x)`** | Log base 10 |

---

## Integer math (no package needed)

For **integers**, you already have:

- **`+`** **`-`** **`*`** **`/`** **`%`** (remainder)
- **`min`**, **`max`** (built-in, Go 1.21+)

Division of integers is **integer division**: **`7 / 2`** is **`3`**, not 3.5. Use **`float64(7)/2`** or **`7.0/2`** if you want a float result.

---

## Summary

- **Numbers in Go have no methods** – use **operators** and the **`math`** package.
- **Built-in:** **`min(...)`**, **`max(...)`** (Go 1.21+).
- **math package:** **Abs**, **Sqrt**, **Pow**, **Floor**, **Ceil**, **Round**, **Min**, **Max**, **Mod**, **Sin**, **Cos**, **Log**, etc.
- **Type:** Most **math** functions take **float64**; convert int with **`float64(n)`** when needed.

**← [Back to INDEX](INDEX.md)** | Next: [21-strings.md](21-strings.md) – **Strings** and the **strings** package.
