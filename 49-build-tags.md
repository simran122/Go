# Build tags and conditional compilation in Go

**← [Back to INDEX](INDEX.md)**

**Build tags** (also called **build constraints**) let you **include or exclude** files (or parts of code) when building. For example, you can have one implementation for **Linux** and another for **Windows**, or code that runs only when a tag like **`debug`** is set. This page explains the basics in simple language.

---

## What are build tags?

- **Build tags** are special **comments** at the top of a file (before **`package`**) that tell the Go tool when to compile that file.
- If the **tag condition** is **true**, the file is compiled; otherwise it is **ignored**.
- So you can keep **platform-specific** or **optional** code in separate files and choose them at build time.

---

## Syntax at the top of a file

Use a comment that starts with **`//go:build`** (or the older **`// +build`**). The file is included only when the expression after it is true.

**Single tag – include file only on Linux:**

```go
//go:build linux

package main

func init() {
    // Linux-specific setup
}
```

**Multiple tags – file is built for Linux OR darwin (macOS):**

```go
//go:build linux || darwin

package mypkg
```

**Negation – file is built on every OS except Windows:**

```go
//go:build !windows

package mypkg
```

---

## Running a build with a tag

Use **`-tags`** to turn on a tag:

```bash
go build -tags debug
```

Then any file with **`//go:build debug`** (and no other unmet conditions) will be included. Without **`-tags debug`**, those files are skipped.

---

## Common uses

- **Operating system** – **`//go:build linux`**, **`//go:build windows`**, **`//go:build darwin`** so each OS gets the right implementation.
- **Optional features** – **`//go:build dev`** or **`//go:build debug`** for code you don’t want in production.
- **Cgo** – **`//go:build cgo`** for files that use cgo, and build with **`-tags cgo`** when you want cgo.

---

## Summary

| Idea | Example |
|------|---------|
| Include file when tag is set | **`//go:build tagName`** at top of file (before **package**) |
| OR | **`//go:build linux \|\| darwin`** |
| NOT | **`//go:build !windows`** |
| Build with tag | **`go build -tags debug`** |

Build tags are **advanced**; use them when you need different code for different platforms or build configurations.

**← [Back to INDEX](INDEX.md)** | Prev: [48-cgo.md](48-cgo.md).
