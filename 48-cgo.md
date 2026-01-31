# cgo – calling C code from Go

**← [Back to INDEX](INDEX.md)**

**cgo** lets you call **C code** from Go and (with care) C code can call Go. It is **advanced** and used when you need to use existing C libraries or system APIs. This page gives a very short overview so you know it exists; real use requires C and build-tool experience.

---

## What is cgo?

- **cgo** is a Go tool that compiles code that uses **`import "C"`** together with **C** code.
- You write **C** in comments above **`import "C"`** or in separate **.c** files, and call that C from Go.
- The Go toolchain invokes a **C compiler** (e.g. **gcc**, **clang**) to build the C parts.

---

## Minimal example

```go
package main

// #include <stdio.h>
// void sayHello() { printf("Hello from C\n"); }
import "C"

func main() {
    C.sayHello()  // call C function
}
```

- The **comment** above **`import "C"`** is **C code** (here: include and a function).
- In Go you call **C.sayHello()**.
- Build with **`go build`** (cgo is enabled by default when the compiler sees **`import "C"`**).

---

## Why use cgo?

- **Existing C libraries** – use a library that only has a C API.
- **System APIs** – some OS or hardware interfaces are C-only.
- **Performance** – rarely needed; Go is usually fast enough. Use only when you have a measured need and a C library that helps.

---

## Downsides

- **Build complexity** – requires a C compiler and correct environment (e.g. **CC**, **CGO_ENABLED**).
- **Cross-compilation** – harder; often you need **CGO_ENABLED=0** for pure Go cross-builds.
- **Safety** – C code can break memory safety; use with care.
- **Portability** – not all platforms support cgo equally.

---

## Summary

| Idea | Example |
|------|---------|
| Use C from Go | **`import "C"`** with C code in comment above it; call **C.funcName()** |
| When to use | Existing C libraries, system APIs, rare performance needs |
| Cost | Needs C compiler, harder cross-compilation, more complexity |

For most Go programs you **don’t need cgo**. Use it only when you must talk to C.

**← [Back to INDEX](INDEX.md)** | Prev: [47-reflection.md](47-reflection.md) | Next: [49-build-tags.md](49-build-tags.md) – **Build tags**.
