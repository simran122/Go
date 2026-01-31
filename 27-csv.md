# CSV in Go (`encoding/csv`)

**← [Back to INDEX](INDEX.md)**

CSV files have **rows** and **columns** (often separated by commas). Use **`encoding/csv`** to read and write them. Everything is in simple language.

---

## Read CSV

**`csv.NewReader(reader)`** creates a CSV reader. **`Read()`** returns one row at a time (a **slice of strings**). **`ReadAll()`** returns **all** rows at once.

```go
package main

import (
    "encoding/csv"
    "fmt"
    "os"
)

func main() {
    file, err := os.Open("data.csv")
    if err != nil {
        panic(err)
    }
    defer file.Close()

    reader := csv.NewReader(file)
    rows, err := reader.ReadAll()
    if err != nil {
        panic(err)
    }
    for _, row := range rows {
        fmt.Println(row)  // each row is []string{"col1", "col2", ...}
    }
}
```

- **`row`** is **`[]string`** – one string per column.

---

## Write CSV

**`csv.NewWriter(writer)`** creates a CSV writer. **`Write(row)`** writes one row (a **slice of strings**). Call **`Flush()`** when you are done.

```go
file, _ := os.Create("output.csv")
defer file.Close()

writer := csv.NewWriter(file)
writer.Write([]string{"Name", "Age", "City"})
writer.Write([]string{"Alice", "25", "NYC"})
writer.Write([]string{"Bob", "30", "LA"})
writer.Flush()
```

---

## Summary

| Task | Function |
|------|----------|
| Read CSV | **`csv.NewReader(file)`** + **`ReadAll()`** |
| Write CSV | **`csv.NewWriter(file)`** + **`Write(row)`** + **`Flush()`** |

**← [Back to INDEX](INDEX.md)** | Next: [28-sort.md](28-sort.md) – **Sort**: sort slices (ints, strings, custom).
