# Go modules (go mod) – full walkthrough

**← [Back to INDEX](INDEX.md)**

**Go modules** are the standard way to manage **dependencies** and **versions** in Go. A **module** is a folder that contains a **`go.mod`** file. This page walks you through **init**, **get**, **tidy**, and **why** you use them.

---

## Why use go mod?

- **Dependencies** – your code can **import** packages from the internet (e.g. GitHub). Go downloads them and records versions in **`go.mod`** and **`go.sum`**.
- **Reproducible builds** – **`go.sum`** stores exact checksums so everyone gets the same dependency versions.
- **No GOPATH** – with modules, you can put your project **anywhere**; you don’t need a special directory.

---

## go mod init – start a new module

From your **project root** (the folder that will contain your Go code), run:

```bash
go mod init example.com/myapp
```

This creates **`go.mod`** with a **module path** (e.g. **`example.com/myapp`**). Use a path that looks like a URL you “own” (e.g. **`github.com/yourname/myapp`** for a real project).

**Example `go.mod` after init:**

```go
module example.com/myapp

go 1.22
```

---

## Writing code and adding dependencies

When you **import** a package in your code (e.g. **`import "github.com/some/lib"`**) and run **`go build`** or **`go run`**, Go will **download** that package and add it to **`go.mod`** (and **`go.sum`**).

You can also add or upgrade a dependency by hand:

```bash
go get github.com/some/package@v1.2.3   # specific version
go get github.com/some/package@latest   # latest
go get -u ./...                         # upgrade all dependencies in current module
```

---

## go mod tidy

**`go mod tidy`** cleans **`go.mod`** and **`go.sum`**:

- **Removes** dependencies that are no longer used in your code.
- **Adds** any missing dependencies required by your code.
- Updates **`go.sum`** with checksums for all direct and indirect dependencies.

Run it regularly (e.g. after removing an import):

```bash
go mod tidy
```

---

## go.mod and go.sum

- **`go.mod`** – lists the **module path**, **Go version**, and **direct dependencies** (and their versions). You can edit it by hand in some cases, but **`go get`** and **`go mod tidy`** usually do the work.
- **`go.sum`** – list of **checksums** of dependency files. Used to verify that the same code is downloaded every time. **Commit both** **`go.mod`** and **`go.sum`** to version control.

---

## Typical workflow

1. **Create project folder** and **`cd`** into it.
2. **`go mod init <module-path>`** – create the module.
3. **Write code** and **import** packages as needed.
4. **`go build`** or **`go run .`** – Go will download dependencies and update **`go.mod`** / **`go.sum`**.
5. **`go mod tidy`** – remove unused deps, add missing ones.
6. **Commit** **`go.mod`** and **`go.sum`**.

---

## Summary

| Command / file | Purpose |
|----------------|---------|
| **go mod init &lt;path&gt;** | Create a new module and **`go.mod`** in the current directory |
| **go get &lt;pkg&gt;@&lt;version&gt;** | Add or upgrade a dependency; updates **`go.mod`** and **`go.sum`** |
| **go mod tidy** | Remove unused deps, add missing ones, update **`go.sum`** |
| **go.mod** | Module path, Go version, direct dependencies |
| **go.sum** | Checksums for reproducible builds; commit with **go.mod** |

For packages and imports in code, see [15-packages.md](15-packages.md).

**← [Back to INDEX](INDEX.md)** | Prev: [44-new-vs-make.md](44-new-vs-make.md) | Next: [46-benchmarking.md](46-benchmarking.md) – **Benchmarking**.
