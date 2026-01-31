# Packages in Go

**← [Back to INDEX](INDEX.md)**

A **package** is a folder of Go files that belong together. Each file in that folder starts with **`package packageName`**. Packages help you **organize** code and **reuse** it (your own or from others).

---

## Why use packages?

- **Organize** – group related code (e.g. “math”, “strings”, “user”).
- **Reuse** – use code from the same project or from the internet (e.g. **import**).
- **Namespaces** – names live inside a package, so different packages can use the same name (e.g. **math.Abs** vs **user.Abs**).

---

## The `main` package

A program you **run** must have a **`main`** package and a **`func main()`** in it. That’s the entry point.

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello")
}
```

**`package main`** – this file is part of the main program.  
**`import "fmt"`** – we use the **fmt** package (from the standard library) to print.

---

## Importing packages

Use **`import`** to use another package. You can use a single package or a group.

**Single import:**

```go
import "fmt"
```

**Multiple imports (group):**

```go
import (
    "fmt"
    "math"
)
```

Then you call: **`fmt.Println(...)`**, **`math.Sqrt(4)`**, etc. The **package name** (e.g. **fmt**, **math**) is the last part of the path (by convention).

---

## Your own package

Create a **folder** (e.g. **mylib**) and put Go files in it. Each file must start with the **same package name** (usually the folder name).

**Example layout:**

```
myproject/
  go.mod
  main.go          ← package main
  mylib/
    mylib.go       ← package mylib
```

**mylib/mylib.go:**

```go
package mylib

func Add(a, b int) int {
    return a + b
}
```

**main.go:**

```go
package main

import (
    "fmt"
    "myproject/mylib"   // path from go.mod module name
)

func main() {
    sum := mylib.Add(3, 5)
    fmt.Println(sum)  // 8
}
```

**Rule:** Names that start with a **capital letter** (e.g. **Add**) are **exported** – other packages can use them. Names that start with a **lowercase letter** are **private** to the package.

---

## The `go.mod` file

A **module** is a collection of packages with a **name** and **version**. The **go.mod** file defines the module.

```go
module myproject

go 1.21
```

Then import paths are based on **myproject**: e.g. **"myproject/mylib"**.

---

## Summary

| Idea | Example |
|------|---------|
| Entry point | `package main` and `func main()` |
| Import | `import "fmt"` or `import "myproject/mylib"` |
| Use | `fmt.Println()`, `mylib.Add()` |
| Export | Capital name = exported (public), lowercase = private |
| Module | Defined in **go.mod** (e.g. `module myproject`) |

**← [Back to INDEX](INDEX.md)** | Next: [16-error-handling.md](16-error-handling.md) – **Error handling**: how Go handles errors.
