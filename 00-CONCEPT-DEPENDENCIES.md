# Go docs – Where each complex topic is introduced and used

**← [Back to INDEX](INDEX.md)**

This page lists **every** topic that is **introduced** in one file and **used later** in others. Read topics in INDEX order (01 → 02 → … → 49) so you always see a concept **before** it is used.

---

## Core language (basics first, then used everywhere)

| Topic | Introduced in | Used later in |
|-------|----------------|----------------|
| **Variables, types, operators** | 01–05 | Every file after (06–49) |
| **Conditionals (if, switch)** | 06 | Every file after |
| **Loops (for, range)** | 07 | 09, 10, 21, 28, 29, 32, 43, 46, and most later files |
| **Functions (params, return, variadic)** | 08 | Every file after; variadic used in fmt, log, etc. |

---

## Data structures (introduced once, then reused)

| Topic | Introduced in | Used later in |
|-------|----------------|----------------|
| **Arrays and slices** | 09 | 21, 22, 25, 26, 28, 29, 32, 44, 46; and 21-strings, 26-json, 28-sort, 29-bytes, 40-io ([]byte), etc. |
| **Maps** | 10 | 26-json, 36-net-url, 44-new-vs-make |
| **Structs** | 11 | 13-methods, 14-interfaces, 24-oop-in-go, 26-json, 28-sort, 44-new-vs-make, 47-reflection |

---

## Pointers, methods, interfaces (order matters: 11 → 12 → 13 → 14)

| Topic | Introduced in | Used later in |
|-------|----------------|----------------|
| **Pointers** | 12-pointers | 33-flag, 44-new-vs-make, 47-reflection |
| **Methods** | 13-methods | 14-interfaces, 24-oop-in-go |
| **Interfaces** | 14-interfaces | 24-oop-in-go, 40-io (Reader, Writer), 47-reflection |

---

## Packages and errors

| Topic | Introduced in | Used later in |
|-------|----------------|----------------|
| **Packages and import** | 15-packages | Every file that uses other packages; 45-go-modules builds on this (go mod) |
| **Error handling (return error, check err)** | 16-error-handling | 25, 26, 27, 30, 31, 32, 35, 36, 37, 39, 40, 41, 45 (any file that does I/O or parsing) |

---

## Concurrency (17 → 18, then used in 38, 41, 42)

| Topic | Introduced in | Used later in |
|-------|----------------|----------------|
| **Goroutines** | 17-goroutines | 18-channels, 38-sync, 41-context, 42-closures (in examples) |
| **Channels** | 18-channels | 38-sync, 41-context |
| **select** | 18-channels | 41-context |

---

## Defer, panic, recover (19, then used in many)

| Topic | Introduced in | Used later in |
|-------|----------------|----------------|
| **Defer** | 19-defer-panic-recover | 25-file-reading-writing, 34-log, 35-net-http, 38-sync, 40-io, 41-context, 42-closures (in examples) |
| **Panic** | 19-defer-panic-recover | 25, 34-log, 35, 36-net-url, 37-encoding-hex, 40-io (in examples) |
| **Recover** | 19-defer-panic-recover | Used when you need to catch panics (advanced) |

---

## Generics and OOP

| Topic | Introduced in | Used later in |
|-------|----------------|----------------|
| **Generics (type parameters)** | 20-generics | 28-sort (sort.Slice uses a function; slices package uses generics); advanced use in other libs |
| **OOP (struct + methods + interface)** | 24-oop-in-go | Summarizes 11, 13, 14; used when you structure larger apps |

---

## Strings, numbers, conversion

| Topic | Introduced in | Used later in |
|-------|----------------|----------------|
| **Strings, type conversion, []byte** | 21-strings, 22-type-conversion, 29-bytes | 26-json, 31-base64, 32-regexp, 37-hex, 40-io, 43-runes-unicode (string/[]byte/rune) |

---

## Context (41, then used in HTTP)

| Topic | Introduced in | Used later in |
|-------|----------------|----------------|
| **Context (cancel, timeout)** | 41-context | 35-net-http (HTTP client with context) |

---

## Optional / advanced (build on everything before)

| Topic | Introduced in | Used later in |
|-------|----------------|----------------|
| **Closures** | 42-closures | Uses 08-functions; examples use 17-goroutines, 19-defer |
| **Runes and Unicode** | 43-runes-unicode | Uses 07-range, 21-strings, 22-type-conversion |
| **new vs make** | 44-new-vs-make | Uses 09-slices, 10-maps, 11-structs, 12-pointers, 18-channels |
| **Go modules** | 45-go-modules | Uses 15-packages (import, module path) |
| **Benchmarking** | 46-benchmarking | Uses 08-functions, 09-slices (in examples) |
| **Reflection** | 47-reflection | Uses 11-structs, 12-pointers, 14-interfaces |
| **cgo** | 48-cgo | Advanced; no later doc depends on it |
| **Build tags** | 49-build-tags | Advanced; no later doc depends on it |

---

## Summary rule

**Read the INDEX in order (01 → 49).** For every complex topic, it is **introduced** in one file and **used later** only in files that come after it. When a page uses something from another topic, we add a **“Concepts used in this page”** line at the top with links – read those first if you haven’t.

**← [Back to INDEX](INDEX.md)**
