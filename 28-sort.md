# Sort in Go (`sort` package)

**← [Back to INDEX](INDEX.md)**

The **`sort`** package sorts **slices**. You can sort **ints**, **strings**, or **custom types** (with **sort.Slice**). Everything is in simple language.

---

## Sort ints and strings

**`sort.Ints(slice)`** sorts a **slice of ints** in place. **`sort.Strings(slice)`** sorts a **slice of strings** in place.

```go
package main

import (
    "fmt"
    "sort"
)

func main() {
    nums := []int{3, 1, 4, 1, 5}
    sort.Ints(nums)
    fmt.Println(nums)  // [1 1 3 4 5]

    names := []string{"Charlie", "Alice", "Bob"}
    sort.Strings(names)
    fmt.Println(names)  // [Alice Bob Charlie]
}
```

---

## Sort with custom order: `sort.Slice`

**`sort.Slice(slice, less)`** sorts a slice using a **comparison function** you provide. **`less(i, j int) bool`** should return **true** if element **i** should come **before** element **j**.

```go
type Person struct {
    Name string
    Age  int
}

people := []Person{
    {"Bob", 30},
    {"Alice", 25},
    {"Charlie", 28},
}
sort.Slice(people, func(i, j int) bool {
    return people[i].Age < people[j].Age  // sort by age
})
// Now: Alice(25), Charlie(28), Bob(30)
```

---

## Check if already sorted

**`sort.IntsAreSorted(nums)`** and **`sort.StringsAreSorted(names)`** return **true** if the slice is already sorted.

```go
fmt.Println(sort.IntsAreSorted([]int{1, 2, 3}))  // true
fmt.Println(sort.IntsAreSorted([]int{3, 1, 2}))  // false
```

---

## Summary

| Task | Function |
|------|----------|
| Sort ints | **`sort.Ints(slice)`** |
| Sort strings | **`sort.Strings(slice)`** |
| Sort custom | **`sort.Slice(slice, less)`** |
| Check sorted | **`sort.IntsAreSorted`**, **`sort.StringsAreSorted`** |

**← [Back to INDEX](INDEX.md)** | Next: [29-bytes.md](29-bytes.md) – **Bytes**: work with **[]byte** like strings.
