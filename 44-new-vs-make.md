# new vs make in Go

**← [Back to INDEX](INDEX.md)**

**`new`** and **`make`** both **create** something in Go, but they do **different** jobs. **`new`** gives you a **pointer** to a new value that starts at zero (or empty). **`make`** is **only** for **slices**, **maps**, and **channels** – it creates them and **sets them up** so you can use them right away. This page explains both in simple words.

**You need to know first:** This page uses **pointers** ([12-pointers.md](12-pointers.md)), **slices** ([09-arrays-and-slices.md](09-arrays-and-slices.md)), **maps** ([10-maps.md](10-maps.md)), **structs** ([11-structs.md](11-structs.md)), and **channels** ([18-channels.md](18-channels.md)). Read those before this if you haven’t.

---

## new(T)

**`new(T)`** does two things:

1. It **reserves space in memory** for one value of type **T**.
2. It sets that value to the **zero value** (0 for numbers, empty string for string, nil for pointers, etc.).
3. It **returns a pointer** **`*T`** that points to that value.

So you get a **pointer** to a new, zero value. You use **`new`** when you want a pointer to a **struct**, **int**, or any type – but **not** for slices, maps, or channels (for those, use **`make`**).

```go
p := new(int)     // p is *int, *p is 0
s := new(struct{ Name string })  // s points to a struct; s.Name is ""
fmt.Println(*p)   // 0
fmt.Println(s.Name)  // ""
```

**In short:** **`new(T)`** → pointer to a new zero value of **T**.

---

## make(T, ...)

**`make`** is **only** for **slices**, **maps**, and **channels**. It **creates** them and **sets them up** so they are ready to use (e.g. a slice has a backing array, a map is ready for keys, a channel is ready to send/receive). It **returns the value** (slice, map, or channel), **not** a pointer.

- **Slice:** **`make([]T, len)`** or **`make([]T, len, cap)`** – a slice you can use (e.g. 5 ints, all 0).
- **Map:** **`make(map[K]V)`** or **`make(map[K]V, size)`** – an empty map you can add keys to.
- **Channel:** **`make(chan T)`** or **`make(chan T, n)`** – a channel you can send and receive on.

```go
sl := make([]int, 5)       // slice of 5 zeros, length 5
mp := make(map[string]int) // empty map, ready to use
ch := make(chan int)       // unbuffered channel
chBuf := make(chan int, 10) // buffered channel, can hold 10 values
```

**In short:** **`make`** → a ready-to-use slice, map, or channel.

---

## Quick comparison

| | **new(T)** | **make(T, ...)** |
|--|------------|------------------|
| **Used for** | Any type **T** (struct, int, etc.) | Only **slice**, **map**, **channel** |
| **Returns** | **`*T`** (pointer to zero value) | **T** (the value itself, ready to use) |
| **Result** | One new value in memory; you get a pointer to it | A slice/map/channel that is set up and ready |

---

## When to use which?

- **Pointer to a struct or int (or any type)?** → Use **`new(T)`** or **`&MyStruct{}`** (for structs, **`&MyStruct{}`** is very common).
- **Slice?** → Use **`make([]T, len)`** or **`make([]T, len, cap)`** (see [09-arrays-and-slices.md](09-arrays-and-slices.md)).
- **Map?** → Use **`make(map[K]V)`** (see [10-maps.md](10-maps.md)).
- **Channel?** → Use **`make(chan T)`** or **`make(chan T, n)`** (see [18-channels.md](18-channels.md)).

**Rule of thumb:**

- You **cannot** use **`make`** for a struct or int – use **`new`** or **`&T{}`** for those.
- You **cannot** use **`new`** to get a useful slice, map, or channel – use **`make`** for those.

---

## Summary

| Idea | Example |
|------|---------|
| **new(T)** | Reserves space for one value of type **T**, sets it to zero, returns **`*T`**. Use when you want a pointer to any type. |
| **make(slice)** | **`make([]T, len [, cap])`** – creates a slice ready to use. |
| **make(map)** | **`make(map[K]V)`** – creates an empty map ready to use. |
| **make(channel)** | **`make(chan T [, n])`** – creates a channel ready to use. |

**← [Back to INDEX](INDEX.md)** | Prev: [43-runes-unicode.md](43-runes-unicode.md) | Next: [45-go-modules.md](45-go-modules.md) – **Go modules**.
