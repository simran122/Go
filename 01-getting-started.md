# Getting Started with Go

**← [Back to INDEX](INDEX.md)**

This page helps you **install Go** and write your **first program** (Hello, World!).

---

## What you need

- A computer (Windows, Mac, or Linux)
- A text editor (e.g. Notepad, VS Code, or any editor you like)
- A terminal or command prompt to run commands

---

## Step 1: Install Go

1. Go to the official site: [https://go.dev/dl/](https://go.dev/dl/)
2. Download the installer for your operating system.
3. Run the installer and follow the steps.
4. When it’s done, open a **new** terminal and type:

   ```bash
   go version
   ```

   You should see something like: `go version go1.21.0 ...`

If you see a version number, Go is installed correctly.

---

## Step 2: Your first program – Hello, World!

We will write a small program that prints **"Hello, world."** on the screen.

### Create a folder and file

1. Create a folder for your project, for example: `my-first-go`
2. Inside that folder, create a file named `hello.go`

### Write this code in `hello.go`

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, world.")
}
```

### What each part does (in simple words)

- **`package main`** – This file is part of the “main” package. A program that you run must have a `main` package.
- **`import "fmt"`** – We use the “fmt” package to print text. “fmt” stands for “format.”
- **`func main()`** – This is the **main function**. When you run the program, Go starts here.
- **`fmt.Println("Hello, world.")`** – This line prints the text `Hello, world.` and then a new line.

---

## Step 3: Run your program

1. Open a terminal.
2. Go to the folder where `hello.go` is (for example: `cd my-first-go`).
3. Run:

   ```bash
   go run hello.go
   ```

You should see:

```
Hello, world.
```

If you see that, **congratulations** – you just ran your first Go program.

---

## Another way to run: build then run

- **`go run hello.go`** – Compiles and runs in one step (good for learning).
- **`go build hello.go`** – Creates an executable file (e.g. `hello` on Mac/Linux, `hello.exe` on Windows). You can then run that file directly.

---

## Summary

| Step | What you did |
|------|----------------|
| 1 | Installed Go and checked with `go version` |
| 2 | Created `hello.go` with `package main`, `import "fmt"`, and `func main()` |
| 3 | Ran the program with `go run hello.go` |

**← [Back to INDEX](INDEX.md)** | Next: [02-variables.md](02-variables.md) – Learn how to store data in **variables**.
