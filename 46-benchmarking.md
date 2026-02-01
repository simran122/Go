# Benchmarking in Go (go test -bench)

**← [Back to INDEX](INDEX.md)**

**Benchmarking** in Go measures how **fast** a piece of code runs. You write **benchmark functions** in a **`_test.go`** file and run them with **`go test -bench`**. This page shows the basics in simple language.

---

## Why benchmark?

- **Compare** different ways to do the same thing (e.g. which function is faster).
- **Find** slow parts of your program before you optimize.
- **Regress** – re-run benchmarks after changes to see if performance got worse.

---

## Writing a benchmark

Benchmarks live in **`*_test.go`** files, like tests. A benchmark function has the form:

**`func BenchmarkXxx(b *testing.B)`**

Inside the function you run the code you want to measure **`b.N`** times. The test framework sets **`b.N`** so that the run takes long enough to measure.

```go
package mypkg

import "testing"

func BenchmarkSum(b *testing.B) {
    nums := []int{1, 2, 3, 4, 5}
    for i := 0; i < b.N; i++ {
        sum(nums)  // function you want to benchmark
    }
}
```

You **must** run the code **`b.N`** times in a loop; the framework uses that to compute iterations per second.

---

## Running benchmarks

From the directory that contains the test file:

```bash
go test -bench=.
```

- **`-bench=.`** – run all benchmarks in the current package.
- **`-bench=BenchmarkSum`** – run only the benchmark named **BenchmarkSum**.
- **`-benchmem`** – also report memory allocations (useful to see allocations per op).

Example:

```bash
go test -bench=BenchmarkSum -benchmem
```

Example output:

```
BenchmarkSum-8   10000000   120 ns/op   48 B/op   1 allocs/op
```

Meaning: **BenchmarkSum** ran **10000000** times; about **120 ns per op**; **48 B** allocated per op; **1** allocation per op (when **-benchmem** is used).

---

## A full example

```go
// mypkg.go
package mypkg

func sum(nums []int) int {
    total := 0
    for _, n := range nums {
        total += n
    }
    return total
}
```

```go
// mypkg_test.go
package mypkg

import "testing"

func BenchmarkSum(b *testing.B) {
    nums := []int{1, 2, 3, 4, 5}
    for i := 0; i < b.N; i++ {
        sum(nums)
    }
}
```

Run: **`go test -bench=BenchmarkSum -benchmem`**.

---

## Sub-benchmarks (comparing variants)

You can run several variants (e.g. different slice sizes) with **`b.Run`**:

```go
package mypkg

import (
    "fmt"
    "testing"
)

func BenchmarkSumSizes(b *testing.B) {
    sizes := []int{10, 100, 1000}
    for _, size := range sizes {
        nums := make([]int, size)
        for i := range nums {
            nums[i] = i
        }
        b.Run(fmt.Sprintf("size=%d", size), func(b *testing.B) {
            for i := 0; i < b.N; i++ {
                sum(nums)
            }
        })
    }
}
```

Run with **`go test -bench=BenchmarkSumSizes`** to see results per size.

---

## Summary

| Idea | Example |
|------|---------|
| Benchmark function | **`func BenchmarkXxx(b *testing.B)`** in **`*_test.go`** |
| Loop | **`for i := 0; i < b.N; i++ { ... }`** – run code **b.N** times |
| Run all benchmarks | **`go test -bench=.`** |
| Run one benchmark | **`go test -bench=BenchmarkSum`** |
| Show allocations | **`go test -bench=. -benchmem`** |
| Sub-benchmarks | **`b.Run("name", func(b *testing.B) { ... })`** |

**← [Back to INDEX](INDEX.md)** | Prev: [45-go-modules.md](45-go-modules.md) | Next: [47-reflection.md](47-reflection.md) – **Reflection**.
