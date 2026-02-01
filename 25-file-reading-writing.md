# File Reading and Writing in Go

**← [Back to INDEX](INDEX.md)**

This page shows **useful packages** for **reading** and **writing** files in Go. You will use **`os`** and **`bufio`** a lot. Everything is in simple language.

**Concepts used in this page:** We use **defer** ([19-defer-panic-recover.md](19-defer-panic-recover.md)) to close files and **panic** ([19-defer-panic-recover.md](19-defer-panic-recover.md)) in some examples. Read that first if you haven’t.

---

## Packages you need

| Package | What it does |
|---------|----------------|
| **`os`** | Open, create, read, and write files; work with paths and environment |
| **`bufio`** | Buffered reading and writing (good for line-by-line or big files) |
| **`path/filepath`** | Join paths, get directory, get file name (works on any OS) |

---

## 1. Read a whole file: `os.ReadFile`

**`os.ReadFile(filename)`** reads the **entire** file into memory. It returns **`[]byte`** and an **error**. Always check the error.

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    data, err := os.ReadFile("hello.txt")
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    fmt.Println(string(data))  // convert bytes to string to print
}
```

- **`data`** is **`[]byte`** – the raw bytes of the file.
- Use **`string(data)`** to get a string (e.g. for text files).
- If the file does not exist or you cannot read it, **`err`** will not be **nil**.

---

## 2. Write a whole file: `os.WriteFile`

**`os.WriteFile(filename, data, permission)`** writes **all** the bytes to the file. If the file exists, it is **overwritten**. If it does not exist, it is **created**.

```go
package main

import "os"

func main() {
    text := []byte("Hello, file!\nThis is line 2.")
    err := os.WriteFile("output.txt", text, 0644)
    if err != nil {
        panic(err)
    }
}
```

- **`text`** is **`[]byte`**. Use **`[]byte("your string")`** to convert a string to bytes.
- **`0644`** is a common permission: owner can read/write, others can read. You can use **`0666`** for simple cases (read/write for all).

---

## 3. Open a file for reading or writing: `os.Open` and `os.Create`

When you need **more control** (read/write in parts, or keep the file open), use **`os.Open`** (read only) or **`os.Create`** (write; creates or overwrites).

### Open for reading

**What is `file`?** **`file`** is the **open file** – the place you read from. Type **`*os.File`**. When you are done, **close** it with **`file.Close()`** (or **`defer file.Close()`**).

**What is `buf`?** **`buf`** is a **buffer** – a slice of bytes where **Read** puts the data. You create it with **`make([]byte, size)`** and then pass it to **`file.Read(buf)`**; **Read** fills **buf** and tells you how many bytes it read.

**`os.Open(filename)`** opens the file for **reading**. You get an **`*os.File`**. When you are done, **close** it with **`file.Close()`** (or use **`defer file.Close()`**).

```go
package main

import (
    "fmt"
    "io"
    "os"
)

func main() {
    file, err := os.Open("hello.txt")
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    defer file.Close()  // close when function returns

    // Read up to 100 bytes
    buf := make([]byte, 100)
    n, err := file.Read(buf)
    if err != nil && err != io.EOF {
        fmt.Println("Error:", err)
        return
    }
    fmt.Println("Read", n, "bytes:", string(buf[:n]))
}
```

- **`file.Read(buf)`** reads into **`buf`** and returns how many bytes were read (**`n`**) and an error.
- **`io.EOF`** means **“end of file”** – there is no more data to read. It is normal when you finish reading the file.

### Create for writing

**`os.Create(filename)`** creates the file (or **overwrites** if it exists) and opens it for **writing**.

```go
file, err := os.Create("output.txt")
if err != nil {
    panic(err)
}
defer file.Close()

file.Write([]byte("Hello, world!\n"))
```

- **`file.Write(data)`** writes bytes to the file. **data** is **`[]byte`** (e.g. **`[]byte("Hello")`**).

---

## 4. Read line by line: `bufio.Scanner`

**`bufio.Scanner`** is the easiest way to read a file **line by line**. You create a scanner from the file, then call **`Scan()`** in a loop and get each line with **`Text()`**.

```go
package main

import (
    "bufio"
    "fmt"
    "os"
)

func main() {
    file, err := os.Open("lines.txt")
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    defer file.Close()

    scanner := bufio.NewScanner(file)
    for scanner.Scan() {
        line := scanner.Text()
        fmt.Println(line)
    }
    if err := scanner.Err(); err != nil {
        fmt.Println("Error:", err)
    }
}
```

- **`scanner.Scan()`** reads the next line; returns **true** if there was a line, **false** when the file is done or on error.
- **`scanner.Text()`** gives you the current line as a **string**.
- **`scanner.Err()`** tells you if something went wrong (e.g. file too big for buffer).

---

## 5. Write with a buffer: `bufio.Writer`

**`bufio.Writer`** writes to a file (or any **io.Writer**) with a buffer. Use **`WriteString`** to write strings, then **`Flush()`** to make sure everything is written to the file.

```go
package main

import (
    "bufio"
    "os"
)

func main() {
    file, err := os.Create("output.txt")
    if err != nil {
        panic(err)
    }
    defer file.Close()

    writer := bufio.NewWriter(file)
    writer.WriteString("Line 1\n")
    writer.WriteString("Line 2\n")
    writer.Flush()  // important: write buffer to file
}
```

- **`Flush()`** sends the buffered data to the file. Call it before closing the file (or when you need data written immediately).

---

## 6. Paths: `path/filepath`

Use **`path/filepath`** to build and split paths so your code works on **Windows, Mac, and Linux**.

| Function | What it does |
|----------|----------------|
| **`filepath.Join(parts...)`** | Join parts into one path (e.g. **"folder"**, **"file.txt"** → **"folder/file.txt"**) |
| **`filepath.Dir(path)`** | Get the directory part (e.g. **"a/b/c.txt"** → **"a/b"**) |
| **`filepath.Base(path)`** | Get the file name (e.g. **"a/b/c.txt"** → **"c.txt"**) |
| **`filepath.Ext(path)`** | Get the extension (e.g. **"file.txt"** → **".txt"**) |

```go
package main

import (
    "fmt"
    "path/filepath"
)

func main() {
    path := filepath.Join("myfolder", "data", "file.txt")
    fmt.Println(path)  // myfolder/data/file.txt (or myfolder\data\file.txt on Windows)

    dir := filepath.Dir(path)
    fmt.Println(dir)   // myfolder/data

    name := filepath.Base(path)
    fmt.Println(name)  // file.txt

    ext := filepath.Ext(path)
    fmt.Println(ext)   // .txt
}
```

---

## 7. Check if a file exists

**`os.Stat(filename)`** returns file info and an error. If the file does not exist, **`err`** will not be **nil** (use **`os.IsNotExist(err)`** to check).

```go
if _, err := os.Stat("myfile.txt"); os.IsNotExist(err) {
    fmt.Println("File does not exist")
} else {
    fmt.Println("File exists")
}
```

---

## 8. Create a directory

**`os.Mkdir(path, permission)`** creates **one** directory. **`os.MkdirAll(path, permission)`** creates the path and **all** parent directories (like **mkdir -p**).

```go
err := os.MkdirAll("myfolder/subfolder", 0755)
if err != nil {
    panic(err)
}
```

---

## 9. Remove a file or directory

**`os.Remove(path)`** removes a file or **empty** directory. **`os.RemoveAll(path)`** removes a directory and **everything inside** it.

```go
os.Remove("file.txt")
os.RemoveAll("myfolder")
```

---

## Summary: file and path topics

| Task | Package / function |
|------|---------------------|
| Read whole file | **`os.ReadFile(filename)`** |
| Write whole file | **`os.WriteFile(filename, data, 0644)`** |
| Open for read | **`os.Open`** + **`file.Read`** + **`defer file.Close()`** |
| Create for write | **`os.Create`** + **`file.Write`** + **`defer file.Close()`** |
| Read line by line | **`bufio.NewScanner(file)`** + **`Scan()`** + **`Text()`** |
| Write with buffer | **`bufio.NewWriter(file)`** + **`WriteString`** + **`Flush()`** |
| Join path | **`filepath.Join(...)`** |
| Dir / Base / Ext | **`filepath.Dir`**, **`filepath.Base`**, **`filepath.Ext`** |
| File exists? | **`os.Stat`** + **`os.IsNotExist(err)`** |
| Create dir(s) | **`os.Mkdir`** or **`os.MkdirAll`** |
| Remove | **`os.Remove`** or **`os.RemoveAll`** |

These are the main packages and functions you need for **file reading, file writing, and paths**. For **manipulating data**, see: [26-json.md](26-json.md) (JSON), [27-csv.md](27-csv.md) (CSV), [28-sort.md](28-sort.md) (sort), [29-bytes.md](29-bytes.md) (bytes).

**← [Back to INDEX](INDEX.md)** | Next: [26-json.md](26-json.md) – **JSON**: Marshal, Unmarshal.
