# Type Conversion (Type Casting) in Go

**← [Back to INDEX](INDEX.md)**

**Type conversion** in Go means turning a value from one type into another: for example **int** to **float64**, or **number** to **string**. Go does **not** use the word “casting” like some other languages; it uses **conversion** with the syntax **`Type(value)`**.

---

## Basic syntax: `Type(value)`

You write the **target type** like a function and put the value in parentheses:

```go
valueOfNewType := TargetType(oldValue)
```

Not every conversion is allowed. The types must be **compatible** (e.g. numeric to numeric, or specific rules for string).

---

## Number to number

Converting between **int** and **float** is common. The result may lose precision (e.g. float → int drops the decimal part).

```go
package main

import "fmt"

func main() {
    // int to float64
    i := 42
    f := float64(i)
    fmt.Println(f)  // 42

    // float64 to int (decimal part is dropped)
    x := 3.14
    n := int(x)
    fmt.Println(n)  // 3

    // int to int (different sizes)
    var a int32 = 100
    b := int64(a)
    fmt.Println(b)  // 100
}
```

**Important:** **`int(3.9)`** gives **`3`** – Go **truncates** toward zero; it does **not** round.

---

## Number to string (and string to number)

For **numbers as text** (e.g. `"42"`, `"3.14"`), use the **`strconv`** package. Do **not** rely only on **`string(65)`** for integers – that gives the **character** with that code (e.g. `"A"`), not the digits `"65"`.

### Integer to string: `strconv.Itoa`

**`strconv.Itoa(i)`** – int to string (decimal digits).

```go
package main

import (
    "fmt"
    "strconv"
)

func main() {
    n := 42
    s := strconv.Itoa(n)
    fmt.Println(s)  // "42"
}
```

### String to integer: `strconv.Atoi`

**`strconv.Atoi(s)`** – string to int. It returns **two values**: the number and an **error**. Always check the error.

```go
s := "42"
n, err := strconv.Atoi(s)
if err != nil {
    fmt.Println("invalid number:", err)
    return
}
fmt.Println(n)  // 42
```

### String to float: `strconv.ParseFloat`

**`strconv.ParseFloat(s, bitSize)`** – string to float. `bitSize` is usually **64** for **float64**.

```go
s := "3.14"
f, err := strconv.ParseFloat(s, 64)
if err != nil {
    fmt.Println("invalid number:", err)
    return
}
fmt.Println(f)  // 3.14
```

### Float to string: `strconv.FormatFloat`

**`strconv.FormatFloat(f, 'f', precision, 64)`** – float64 to string. Use **`'f'`** for normal decimal form.

```go
f := 3.14
s := strconv.FormatFloat(f, 'f', 2, 64)
fmt.Println(s)  // "3.14"
```

---

## String and byte slice

- **`[]byte(s)`** – string to slice of bytes. Good for reading or writing raw bytes.
- **`string(bytes)`** – slice of bytes to string.

```go
s := "hello"
b := []byte(s)   // []byte{'h','e','l','l','o'}
s2 := string(b)  // "hello"
fmt.Println(b, s2)
```

---

## Single character (rune) to string

- **`string(r)`** – one rune (Unicode code point) to string. Example: **`string(65)`** is **`"A"`**, **`string('a')`** is **`"a"`**.

```go
r := 'A'
s := string(r)  // "A"
fmt.Println(s)
```

---

## What you cannot do

- You **cannot** convert **bool** to **int** or **string** with **`int(b)`** or **`string(b)`** in a useful way. Use **if** or **strconv** (e.g. **`strconv.FormatBool`**) instead.
- You **cannot** convert **string** to **int** with **`int("42")`** – use **`strconv.Atoi("42")`**.

---

## Summary

| Conversion | How |
|------------|-----|
| int ↔ float64 | `float64(i)`, `int(f)` (truncates) |
| int → string (digits) | `strconv.Itoa(n)` |
| string → int | `strconv.Atoi(s)` (returns value, error) |
| string → float64 | `strconv.ParseFloat(s, 64)` |
| float64 → string | `strconv.FormatFloat(f, 'f', prec, 64)` |
| string ↔ []byte | `[]byte(s)`, `string(b)` |
| rune → string | `string(r)` |

**← [Back to INDEX](INDEX.md)** | Next: [23-numbers-math.md](23-numbers-math.md) – **Numbers and the math package**.
