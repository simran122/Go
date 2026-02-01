# Go Documentation – Coverage Checklist (Basic to Advanced)

**← [Back to INDEX](INDEX.md)**

This page checks whether this documentation covers **all standard Go topics** from basic to advanced. ✅ = covered in a dedicated file or clearly inside another topic. ⚠️ = partly covered or only mentioned. ❌ = not covered.

---

## Part 1: Basics

| Topic | Status | Where |
|-------|--------|--------|
| Getting started (install, Hello World) | ✅ | 01-getting-started.md |
| Variables (var, :=) | ✅ | 02-variables.md |
| Constants, iota | ✅ | 03-constants.md |
| Basic types (int, float, string, bool) | ✅ | 04-basic-types.md |
| Zero values | ✅ | 04-basic-types.md |
| Operators (arithmetic, comparison, logical) | ✅ | 05-operators.md |

---

## Part 2: Control Flow

| Topic | Status | Where |
|-------|--------|--------|
| if / else / else if | ✅ | 06-conditionals.md |
| switch (expression, no expression) | ✅ | 06-conditionals.md |
| for loop (classic, condition-only) | ✅ | 07-loops.md |
| for range (slice, map, string) | ✅ | 07-loops.md |
| break, continue | ✅ | 07-loops.md |

---

## Part 3: Functions

| Topic | Status | Where |
|-------|--------|--------|
| Functions (params, return) | ✅ | 08-functions.md |
| Multiple return values | ✅ | 08-functions.md |
| Named return values | ✅ | 08-functions.md |
| **Variadic functions (func f(args ...int))** | ✅ | 08-functions.md |
| **Closures (anonymous functions capturing variables)** | ✅ | 42-closures.md |

---

## Part 4: Data Structures

| Topic | Status | Where |
|-------|--------|--------|
| Arrays | ✅ | 09-arrays-and-slices.md |
| Slices (make, append, len, cap) | ✅ | 09-arrays-and-slices.md |
| slices package (Contains, Sort, Clone, Delete) | ✅ | 09-arrays-and-slices.md |
| copy | ✅ | 09-arrays-and-slices.md |
| Maps | ✅ | 10-maps.md |
| Structs | ✅ | 11-structs.md |
| Struct embedding | ✅ | 11-structs.md, 24-oop-in-go.md |

---

## Part 5: Pointers & Types

| Topic | Status | Where |
|-------|--------|--------|
| Pointers (&, *, new) | ✅ | 12-pointers.md |
| Passing pointers to functions | ✅ | 12-pointers.md |
| nil pointer | ✅ | 12-pointers.md |
| **new vs make** | ✅ | 44-new-vs-make.md |
| Methods (value vs pointer receiver) | ✅ | 13-methods.md |
| Interfaces | ✅ | 14-interfaces.md |
| Type assertion | ✅ | 14-interfaces.md |
| Empty interface (any) | ✅ | 14-interfaces.md |
| **Type switch (switch v := x.(type))** | ✅ | 14-interfaces.md |

---

## Part 6: OOP & Design

| Topic | Status | Where |
|-------|--------|--------|
| “Class” in Go (struct + methods) | ✅ | 24-oop-in-go.md, 11, 13 |
| “Extend” (embedding, composition) | ✅ | 24-oop-in-go.md, 11-structs.md |
| “Static” (package-level var/func) | ✅ | 24-oop-in-go.md |
| “Abstract class” (interface) | ✅ | 24-oop-in-go.md, 14-interfaces.md |
| Packages & imports | ✅ | 15-packages.md |
| **go mod (go.mod, go.sum, go get)** | ✅ | 45-go-modules.md |

---

## Part 7: Error Handling & Advanced Control

| Topic | Status | Where |
|-------|--------|--------|
| Error handling (return error, check err) | ✅ | 16-error-handling.md |
| errors.New, fmt.Errorf | ✅ | 16-error-handling.md, 39-fmt.md |
| Defer | ✅ | 19-defer-panic-recover.md |
| Panic | ✅ | 19-defer-panic-recover.md |
| Recover | ✅ | 19-defer-panic-recover.md |

---

## Part 8: Concurrency

| Topic | Status | Where |
|-------|--------|--------|
| Goroutines | ✅ | 17-goroutines.md |
| sync.WaitGroup | ✅ | 17-goroutines.md, 38-sync.md |
| Channels (unbuffered, buffered) | ✅ | 18-channels.md |
| Send/receive, close, range | ✅ | 18-channels.md |
| select | ✅ | 18-channels.md |
| sync.Mutex | ✅ | 38-sync.md |
| sync.Once | ✅ | 38-sync.md |
| **Context (cancellation, timeout)** | ✅ | 41-context.md |

---

## Part 9: Generics

| Topic | Status | Where |
|-------|--------|--------|
| Type parameters | ✅ | 20-generics.md |
| Constraints (any, interface with union) | ✅ | 20-generics.md |
| Generic functions and types | ✅ | 20-generics.md |

---

## Part 10: Strings, Numbers, Conversion

| Topic | Status | Where |
|-------|--------|--------|
| strings package | ✅ | 21-strings.md |
| Type conversion (casting) | ✅ | 22-type-conversion.md |
| strconv | ✅ | 22-type-conversion.md |
| math package | ✅ | 23-numbers-math.md |
| **Rune type / Unicode in strings** | ✅ | 43-runes-unicode.md |
| bytes package | ✅ | 29-bytes.md |

---

## Part 11: Standard Library (I/O & Data)

| Topic | Status | Where |
|-------|--------|--------|
| os (ReadFile, WriteFile, Open, Create) | ✅ | 25-file-reading-writing.md |
| bufio (Scanner, Writer) | ✅ | 25-file-reading-writing.md |
| path/filepath | ✅ | 25-file-reading-writing.md |
| encoding/json | ✅ | 26-json.md |
| encoding/csv | ✅ | 27-csv.md |
| sort | ✅ | 28-sort.md |
| time | ✅ | 30-time.md |
| encoding/base64 | ✅ | 31-encoding-base64.md |
| encoding/hex | ✅ | 37-encoding-hex.md |
| regexp | ✅ | 32-regexp.md |
| flag | ✅ | 33-flag.md |
| log | ✅ | 34-log.md |
| net/http | ✅ | 35-net-http.md |
| net/url | ✅ | 36-net-url.md |
| fmt (Printf, Sprintf, Errorf, Scanf) | ✅ | 39-fmt.md |
| io (Reader, Writer, ReadAll, Copy) | ✅ | 40-io.md |
| context | ✅ | 41-context.md |

---

## Part 12: Optional / Advanced (now covered)

| Topic | Status | Where |
|-------|--------|------|
| **Testing** | ❌ | Removed by request. |
| **Benchmarking** (go test -bench) | ✅ | 46-benchmarking.md |
| **Variadic functions** | ✅ | 08-functions.md |
| **Closures** (dedicated page) | ✅ | 42-closures.md |
| **Runes & Unicode** (dedicated page) | ✅ | 43-runes-unicode.md |
| **new vs make** (one place) | ✅ | 44-new-vs-make.md |
| **Type switch** | ✅ | 14-interfaces.md |
| **go mod** (full tutorial) | ✅ | 45-go-modules.md |
| **Reflection** (reflect) | ✅ | 47-reflection.md |
| **cgo** | ✅ | 48-cgo.md |
| **Build tags / conditional compilation** | ✅ | 49-build-tags.md |

---

## Summary

| Level | Coverage |
|-------|----------|
| **Basic** (syntax, types, control flow, functions, data structures) | ✅ **Complete** |
| **Intermediate** (pointers, methods, interfaces, packages, errors) | ✅ **Complete** |
| **Advanced** (goroutines, channels, select, defer/panic/recover, generics) | ✅ **Complete** |
| **Standard library** (files, JSON, CSV, time, HTTP, flag, log, io, context, etc.) | ✅ **Complete** |
| **Optional / advanced** | ✅ Variadic (08), closures (42), runes (43), new vs make (44), type switch (14), go mod (45), benchmarking (46), reflection (47), cgo (48), build tags (49). |
| **Intentionally omitted** | ❌ Testing only (by request). |

**Conclusion:** This documentation covers **all core Go topics from basic to advanced**, plus **optional/advanced** topics: variadic functions, closures, runes & Unicode, new vs make, type switch, go modules, benchmarking, reflection, cgo, and build tags. Only **testing** is omitted by request.

**← [Back to INDEX](INDEX.md)**
