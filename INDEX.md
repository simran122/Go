# Go Programming Language – Beginner's Documentation

Welcome! This is a simple, step-by-step guide to learn **Go** (also called Golang) from the very beginning to more advanced topics. Every topic has its own page with easy examples.

**What is Go?**  
Go is a programming language made by Google. It is easy to read, fast to run, and great for building software that does many things at once (concurrency).

**Coverage check:** See [00-COVERAGE-CHECKLIST.md](00-COVERAGE-CHECKLIST.md) for a topic-by-topic checklist (basic to advanced) and what is covered or optional.

**Concept dependencies:** See [00-CONCEPT-DEPENDENCIES.md](00-CONCEPT-DEPENDENCIES.md) for **every** complex topic: where it is **introduced** (first explained) and where it is **used later** (so you read in the right order).

---

## How to use this guide

- Start from **Part 1: Basics** and go in order.
- Each topic has its own file. Open the file, read the explanation, and try the examples.
- Type the examples yourself and run them to learn better.
- **Concepts are introduced before use:** each page only uses ideas from **earlier** pages (e.g. we explain **interfaces** in 14 before using them in 40-io; we explain **defer** in 19 before using it in 25, 34, 35, 38, 40, 41). When a page uses something from another topic, we add a **“Concepts used in this page”** line at the top with links – read those first if you haven’t.

---

**Learning path (like school):** The order is like a child going through classes – **first** you learn the basics (variables, types, operators = like learning 1, 2, 3, 4). **Then** you use them (conditionals, loops, functions = like add, subtract). **Next** you learn data (arrays, maps, structs), then pointers and methods, then interfaces. **Later** you learn advanced control (errors, goroutines, channels, defer). **Finally** you use everything together (files, HTTP, io, context) and optional topics (closures, new vs make, reflection). So the INDEX is in the **correct order** – foundations first, each topic builds on what came before. For a **full list of every complex topic** (where it is introduced and where it is used later), see [00-CONCEPT-DEPENDENCIES.md](00-CONCEPT-DEPENDENCIES.md).

---

## Part 1: Basics

| Topic | File | What you'll learn |
|-------|------|-------------------|
| **Getting Started** | [01-getting-started.md](01-getting-started.md) | Install Go and write your first "Hello, World!" program |
| **Variables** | [02-variables.md](02-variables.md) | Store data in variables and use short declaration |
| **Constants** | [03-constants.md](03-constants.md) | Use values that never change, and `iota` |
| **Basic Types** | [04-basic-types.md](04-basic-types.md) | Numbers, strings, and booleans in Go |
| **Operators** | [05-operators.md](05-operators.md) | Math, comparison, and logical operators |

---

## Part 2: Control Flow

| Topic | File | What you'll learn |
|-------|------|-------------------|
| **Conditionals** | [06-conditionals.md](06-conditionals.md) | `if`, `else`, and `switch` to make decisions |
| **Loops** | [07-loops.md](07-loops.md) | `for` and `range` to repeat actions |
| **Functions** | [08-functions.md](08-functions.md) | Write and call functions, return values |

---

## Part 3: Data Structures

| Topic | File | What you'll learn |
|-------|------|-------------------|
| **Arrays and Slices** | [09-arrays-and-slices.md](09-arrays-and-slices.md) | Lists of data: fixed (arrays) and flexible (slices) |
| **Maps** | [10-maps.md](10-maps.md) | Store key–value pairs like a dictionary |
| **Structs** | [11-structs.md](11-structs.md) | Group related data into custom types (like “objects”) |
| **Objects in Go** | [11-structs.md](11-structs.md) + [13-methods.md](13-methods.md) | In Go, “objects” = **structs** (data) + **methods** (functions on that type) |

---

## Part 4: Intermediate Topics

| Topic | File | What you'll learn |
|-------|------|-------------------|
| **Pointers** | [12-pointers.md](12-pointers.md) | Work with memory addresses and references |
| **Methods** | [13-methods.md](13-methods.md) | Functions that belong to a type (receiver) |
| **Interfaces** | [14-interfaces.md](14-interfaces.md) | Define behavior without fixing the type |
| **Packages** | [15-packages.md](15-packages.md) | Organize code and use other people's code |

---

## Part 5: Advanced Topics

| Topic | File | What you'll learn |
|-------|------|-------------------|
| **Error Handling** | [16-error-handling.md](16-error-handling.md) | Return and check errors the Go way |
| **Goroutines** | [17-goroutines.md](17-goroutines.md) | Run code at the same time (concurrency) |
| **Channels** | [18-channels.md](18-channels.md) | Send and receive data between goroutines |
| **Defer, Panic, and Recover** | [19-defer-panic-recover.md](19-defer-panic-recover.md) | Clean up, handle panics, and recover |
| **Generics** | [20-generics.md](20-generics.md) | Write code that works with different types |

---

## Part 6: Strings, Numbers, and Type Conversion

| Topic | File | What you'll learn |
|-------|------|-------------------|
| **Strings and the `strings` package** | [21-strings.md](21-strings.md) | All common string operations: Contains, Replace, Split, Join, Trim, ToUpper, etc. (Go has no “string methods” – use the `strings` package) |
| **Type conversion (casting)** | [22-type-conversion.md](22-type-conversion.md) | Convert between int, float, string, []byte; use `strconv` for number ↔ string |
| **Numbers and the `math` package** | [23-numbers-math.md](23-numbers-math.md) | Math functions: Abs, Sqrt, Pow, Min, Max, Round; Go numbers have no “methods” – use `math` and built-in `min`/`max` |

**Note:** Arrays and slices **do not have methods** in Go. Use **built-ins** (`len`, `cap`, `append`, `copy`) and the **`slices`** package (Contains, Sort, Clone, Delete, etc.) – see [09-arrays-and-slices.md](09-arrays-and-slices.md).

---

## Part 7: OOP in Go (for Java / C# / JavaScript developers)

| Topic | File | What you'll learn |
|-------|------|-------------------|
| **OOP: class, extend, static, abstract, interface** | [24-oop-in-go.md](24-oop-in-go.md) | How to make a “class” (struct + methods), “extend parent” (embedding), “static” (package-level func/var), “abstract class” (interface), and interfaces in Go |

---

## Part 8: Useful packages (files and data)

| Topic | File | What you'll learn |
|-------|------|-------------------|
| **File reading and writing** | [25-file-reading-writing.md](25-file-reading-writing.md) | **os** (ReadFile, WriteFile, Open, Create), **bufio** (Scanner, Writer), **path/filepath** (Join, Dir, Base), check exists, Mkdir, Remove |
| **JSON** | [26-json.md](26-json.md) | **encoding/json**: Marshal, Unmarshal, MarshalIndent, struct tags |
| **CSV** | [27-csv.md](27-csv.md) | **encoding/csv**: Read, Write, ReadAll, Flush |
| **Sort** | [28-sort.md](28-sort.md) | **sort**: Ints, Strings, Slice (custom order), IntsAreSorted |
| **Bytes** | [29-bytes.md](29-bytes.md) | **bytes**: Contains, Split, Join, ReplaceAll, TrimSpace, Equal; what is []byte, when to use bytes vs strings |
| **Time** | [30-time.md](30-time.md) | **time**: Now, Sleep, Format, Parse, Duration, Since |
| **Encoding Base64** | [31-encoding-base64.md](31-encoding-base64.md) | **encoding/base64**: EncodeToString, DecodeString (binary in JSON/URL) |
| **Regexp** | [32-regexp.md](32-regexp.md) | **regexp**: MatchString, Compile, FindString, FindAllString, ReplaceAllString |
| **More useful packages (index)** | [33-more-useful-packages.md](33-more-useful-packages.md) | Index linking to each package doc below |
| **flag** | [33-flag.md](33-flag.md) | Command-line flags |
| **log** | [34-log.md](34-log.md) | Logging, Fatal |
| **net/http** | [35-net-http.md](35-net-http.md) | HTTP server and client |
| **net/url** | [36-net-url.md](36-net-url.md) | URL parsing, query params |
| **encoding/hex** | [37-encoding-hex.md](37-encoding-hex.md) | Hex encode/decode |
| **sync** | [38-sync.md](38-sync.md) | Mutex, WaitGroup, Once |
| **fmt** | [39-fmt.md](39-fmt.md) | Printf, Sprintf, Errorf, Scanf |
| **io** | [40-io.md](40-io.md) | Reader, Writer, ReadAll, Copy |
| **context** | [41-context.md](41-context.md) | Cancellation, timeouts |

---

## Part 9: More language & tooling

| Topic | File | What you'll learn |
|-------|------|-------------------|
| **Closures** | [42-closures.md](42-closures.md) | Functions that capture variables; anonymous functions; loop + closure gotcha |
| **Runes & Unicode** | [43-runes-unicode.md](43-runes-unicode.md) | rune, UTF-8, range over string, unicode/utf8 |
| **new vs make** | [44-new-vs-make.md](44-new-vs-make.md) | When to use new(T) vs make(slice/map/channel) |
| **Go modules** | [45-go-modules.md](45-go-modules.md) | go mod init, go get, go mod tidy, go.mod, go.sum |
| **Benchmarking** | [46-benchmarking.md](46-benchmarking.md) | go test -bench, benchmark functions, -benchmem |
| **Reflection** | [47-reflection.md](47-reflection.md) | reflect package: TypeOf, ValueOf, struct inspection (advanced) |
| **cgo** | [48-cgo.md](48-cgo.md) | Calling C from Go (advanced) |
| **Build tags** | [49-build-tags.md](49-build-tags.md) | Conditional compilation, //go:build |

---

## Quick links (all topics in one list)

1. [01-getting-started.md](01-getting-started.md) – Getting Started  
2. [02-variables.md](02-variables.md) – Variables  
3. [03-constants.md](03-constants.md) – Constants  
4. [04-basic-types.md](04-basic-types.md) – Basic Types  
5. [05-operators.md](05-operators.md) – Operators  
6. [06-conditionals.md](06-conditionals.md) – Conditionals  
7. [07-loops.md](07-loops.md) – Loops  
8. [08-functions.md](08-functions.md) – Functions  
9. [09-arrays-and-slices.md](09-arrays-and-slices.md) – Arrays and Slices  
10. [10-maps.md](10-maps.md) – Maps  
11. [11-structs.md](11-structs.md) – Structs  
12. [12-pointers.md](12-pointers.md) – Pointers  
13. [13-methods.md](13-methods.md) – Methods  
14. [14-interfaces.md](14-interfaces.md) – Interfaces  
15. [15-packages.md](15-packages.md) – Packages  
16. [16-error-handling.md](16-error-handling.md) – Error Handling  
17. [17-goroutines.md](17-goroutines.md) – Goroutines  
18. [18-channels.md](18-channels.md) – Channels  
19. [19-defer-panic-recover.md](19-defer-panic-recover.md) – Defer, Panic, Recover  
20. [20-generics.md](20-generics.md) – Generics  
21. [21-strings.md](21-strings.md) – Strings and the `strings` package  
22. [22-type-conversion.md](22-type-conversion.md) – Type conversion (casting)  
23. [23-numbers-math.md](23-numbers-math.md) – Numbers and the `math` package  
24. [24-oop-in-go.md](24-oop-in-go.md) – OOP in Go: class, extend, static, abstract, interface  
25. [25-file-reading-writing.md](25-file-reading-writing.md) – File reading and writing (os, bufio, filepath)  
26. [26-json.md](26-json.md) – JSON (encoding/json)  
27. [27-csv.md](27-csv.md) – CSV (encoding/csv)  
28. [28-sort.md](28-sort.md) – Sort (sort package)  
29. [29-bytes.md](29-bytes.md) – Bytes (bytes package)  
30. [30-time.md](30-time.md) – Time (time package)  
31. [31-encoding-base64.md](31-encoding-base64.md) – Encoding Base64  
32. [32-regexp.md](32-regexp.md) – Regexp (regular expressions)  
33. [33-more-useful-packages.md](33-more-useful-packages.md) – More useful packages (index)  
34. [33-flag.md](33-flag.md) – flag  
35. [34-log.md](34-log.md) – log  
36. [35-net-http.md](35-net-http.md) – net/http  
37. [36-net-url.md](36-net-url.md) – net/url  
38. [37-encoding-hex.md](37-encoding-hex.md) – encoding/hex  
39. [38-sync.md](38-sync.md) – sync  
40. [39-fmt.md](39-fmt.md) – fmt  
41. [40-io.md](40-io.md) – io  
42. [41-context.md](41-context.md) – context  
43. [42-closures.md](42-closures.md) – Closures  
44. [43-runes-unicode.md](43-runes-unicode.md) – Runes and Unicode  
45. [44-new-vs-make.md](44-new-vs-make.md) – new vs make  
46. [45-go-modules.md](45-go-modules.md) – Go modules  
47. [46-benchmarking.md](46-benchmarking.md) – Benchmarking  
48. [47-reflection.md](47-reflection.md) – Reflection  
49. [48-cgo.md](48-cgo.md) – cgo  
50. [49-build-tags.md](49-build-tags.md) – Build tags  

---

*This documentation uses simple language and examples. It was created with the help of Context7 MCP server for up-to-date Go documentation.*
