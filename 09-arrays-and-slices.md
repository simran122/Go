# Arrays and Slices in Go

**← [Back to INDEX](INDEX.md)**

**Arrays** and **slices** both hold a **list of values** of the same type. Arrays have a **fixed length**. Slices are **flexible** and used much more often in Go.

---

## Arrays (fixed length)

An **array** has a fixed size. You declare it with the type and length: `[length]type`.

```go
package main

import "fmt"

func main() {
    var ages [3]int
    ages[0] = 25
    ages[1] = 30
    ages[2] = 35
    fmt.Println(ages)  // [25 30 35]
}
```

**Index:** Positions start at **0**. So `ages[0]` is the first element, `ages[1]` the second, and so on.

**Literal (with values):**

```go
fruits := [3]string{"apple", "banana", "cherry"}
fmt.Println(fruits[1])  // banana
```

---

## Slices (flexible length)

A **slice** is like a **view** over a list that can grow or shrink. You don’t give a fixed length. Type is **`[]elementType`**.

**Create with a literal:**

```go
package main

import "fmt"

func main() {
    colors := []string{"red", "green", "blue"}
    fmt.Println(colors)  // [red green blue]
}
```

**Create with `make`:**

```go
// make(sliceType, length)
nums := make([]int, 3)  // slice of 3 integers, all 0
fmt.Println(nums)      // [0 0 0]

// make(sliceType, length, capacity)
nums2 := make([]int, 2, 5)  // length 2, capacity 5
```

---

## Length and capacity

- **`len(slice)`** – number of elements in the slice.
- **`cap(slice)`** – capacity (how many elements the underlying array can hold without reallocating).

```go
package main

import "fmt"

func main() {
    s := make([]int, 3, 6)
    fmt.Println(len(s), cap(s))  // 3 6
}
```

---

## Append to a slice

Use **`append`** to add one or more elements. The slice may get a new backing array if there isn’t enough capacity.

```go
package main

import "fmt"

func main() {
    nums := []int{1, 2, 3}
    nums = append(nums, 4)
    nums = append(nums, 5, 6)
    fmt.Println(nums)  // [1 2 3 4 5 6]
}
```

**Important:** You usually assign the result back: `nums = append(nums, 4)`.

---

## Slicing (getting a part of a slice)

You can take a **sub-slice** with **`[start:end]`**. It includes `start`, and **does not** include `end`.

```go
package main

import "fmt"

func main() {
    letters := []string{"a", "b", "c", "d", "e"}
    part := letters[1:4]  // from index 1 to 3
    fmt.Println(part)    // [b c d]
}
```

- **`letters[:3]`** – from start up to index 3 (excluded).
- **`letters[2:]`** – from index 2 to the end.
- **`letters[:]`** – whole slice.

---

## Zero value of a slice

A slice that is not initialized is **`nil`**. Its length and capacity are 0.

```go
var s []int
fmt.Println(s == nil, len(s), cap(s))  // true 0 0
```

---

## Copy slices: the `copy` built-in

**`copy(dst, src)`** copies elements from **src** into **dst**. It copies up to **min(len(dst), len(src))** elements. It returns how many elements were copied.

```go
package main

import "fmt"

func main() {
    src := []int{1, 2, 3, 4, 5}
    dst := make([]int, 3)
    n := copy(dst, src)
    fmt.Println(dst, n)  // [1 2 3] 3
}
```

---

## The `slices` package (Go 1.21+)

In Go, **arrays and slices do not have “methods”** like in some other languages. You use **built-in functions** (**len**, **cap**, **append**, **copy**) and the **`slices`** package for more operations. Import **`"slices"`**.

### Check if a slice contains a value

**`slices.Contains(slice, value)`** – true if the slice contains the value.

```go
package main

import (
    "fmt"
    "slices"
)

func main() {
    nums := []int{1, 2, 3, 4, 5}
    fmt.Println(slices.Contains(nums, 3))   // true
    fmt.Println(slices.Contains(nums, 10))  // false
}
```

### Sort a slice

**`slices.Sort(slice)`** – sorts the slice **in place** (changes the original slice).

```go
nums := []int{3, 1, 4, 1, 5}
slices.Sort(nums)
fmt.Println(nums)  // [1 1 3 4 5]
```

**`slices.SortFunc(slice, less)`** – sort using a custom comparison function.

### Clone (copy) a slice

**`slices.Clone(slice)`** – returns a **new** slice with the same elements. Changing the clone does not change the original.

```go
original := []int{1, 2, 3}
clone := slices.Clone(original)
clone[0] = 99
fmt.Println(original, clone)  // [1 2 3] [99 2 3]
```

### Delete elements

**`slices.Delete(slice, i, j)`** – removes elements from index **i** up to (but not including) **j**. Returns the new slice (you should use the return value).

```go
nums := []int{1, 2, 3, 4, 5}
nums = slices.Delete(nums, 1, 3)  // remove index 1 and 2
fmt.Println(nums)  // [1 4 5]
```

### Other useful `slices` functions

| Function | What it does |
|----------|----------------|
| **`slices.Index(slice, value)`** | Index of first occurrence of value, or -1 |
| **`slices.Min(slice)`** | Smallest element (Go 1.21+) |
| **`slices.Max(slice)`** | Largest element (Go 1.21+) |
| **`slices.Reverse(slice)`** | Reverse the slice in place |
| **`slices.Insert(slice, i, v...)`** | Insert values at index i |

---

## Summary: arrays and slices

| Concept | Array | Slice |
|--------|--------|--------|
| Type | `[n]T` (fixed n) | `[]T` |
| Length | Fixed | Can grow (e.g. with `append`) |
| Create | `[3]int{1,2,3}` | `[]int{1,2,3}` or `make([]int, len, cap)` |
| Length/cap | `len(a)`, `cap(a)` | `len(s)`, `cap(s)` |
| Copy | – | **`copy(dst, src)`** |
| Contains / Sort / Clone / Delete | – | **`slices`** package |

In Go, **slices do not have methods** – use **built-ins** (**len**, **cap**, **append**, **copy**) and the **`slices`** package.

**← [Back to INDEX](INDEX.md)** | Next: [10-maps.md](10-maps.md) – **Maps**: key–value pairs like a dictionary.
