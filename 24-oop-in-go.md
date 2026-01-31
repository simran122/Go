# OOP in Go: Classes, Inheritance, Static, Abstract, Interface

**← [Back to INDEX](INDEX.md)**

If you come from **Java**, **C#**, or **JavaScript**, you are used to **classes**, **extends**, **static**, and **abstract**. Go does **not** have those keywords, but you can do similar things in a different way. This page explains how.

---

## 1. How to make a “class” in Go

Go has **no** **`class`** keyword. A “class” in Go = **struct** (data) + **methods** (functions on that type).

**In Java/C# you might write:**
```text
class Person {
    String name;
    int age;
    void greet() { ... }
}
```

**In Go you write:**

```go
package main

import "fmt"

// "Class" = struct (data) + methods (behavior)
type Person struct {
    Name string
    Age  int
}

func (p Person) Greet() {
    fmt.Println("Hello, I am", p.Name)
}

func main() {
    p := Person{Name: "Alice", Age: 25}
    p.Greet()  // Hello, I am Alice
}
```

- **Struct** = the “fields” (Name, Age).
- **Methods** = the “methods” (Greet). You attach them with **`func (receiver) Name() { }`**.

So: **to make a “class” in Go: define a struct and add methods to it.** See [11-structs.md](11-structs.md) and [13-methods.md](13-methods.md).

---

## 2. How to “extend” a parent (inheritance) in Go

Go has **no** **`extends`** or **inheritance**. You cannot make a “child class” that inherits fields and methods from a “parent class.” Instead you use **composition**: you **embed** one struct inside another. The inner struct’s fields and methods are **promoted** to the outer struct, so it looks a bit like “having a parent.”

**In Java/C# you might write:**
```text
class Animal { void Speak() { } }
class Dog extends Animal { }
```

**In Go you use embedding:**

```go
package main

import "fmt"

// "Parent" – base type
type Animal struct {
    Name string
}

func (a Animal) Speak() {
    fmt.Println(a.Name, "makes a sound")
}

// "Child" – embed Animal (no "extends", use composition)
type Dog struct {
    Animal  // embed: no field name, so Animal's fields/methods are "promoted"
    Breed   string
}

func main() {
    d := Dog{
        Animal: Animal{Name: "Rex"},
        Breed:  "Lab",
    }
    d.Speak()       // Rex makes a sound – method from "parent"
    fmt.Println(d.Name, d.Breed)  // Rex Lab – Name comes from embedded Animal
}
```

**What is happening:**

- **`Dog`** has an **embedded** **`Animal`** (no field name, just the type **`Animal`**).
- Because it is embedded, **`Animal`’s** fields and methods are **promoted**: you can write **`d.Name`** and **`d.Speak()`** instead of **`d.Animal.Name`** and **`d.Animal.Speak()`**.
- So **Dog** “has” **Animal** inside it and reuses its behavior – but it is **composition**, not inheritance. There is no **super** or **override** keyword.

**Override-style behavior:** Define a method with the **same name** on **Dog**. When you call **`d.Speak()`**, Go uses **Dog’s** method, not **Animal’s**.

```go
func (d Dog) Speak() {
    fmt.Println(d.Name, "says: Woof!")
}
```

So: **“extend parent” in Go = embed a struct (composition) and optionally define methods with the same name to “override.”**

---

## 3. How to make “static” in Go

Go has **no** **`static`** keyword. “Static” means: **not tied to an instance** – you can call it without a value of the type.

**In Go:**

- **Static-like “methods”** = **package-level functions** (no receiver). You call them as **`PackageName.FunctionName()`**.
- **Static-like “fields”** = **package-level variables**. They are shared by the whole package (and other packages if the name starts with a capital letter).

**Example: “static” counter and “static” helper**

```go
package main

import "fmt"

var callCount int  // "static" variable – shared, not on a single instance

func GetCallCount() int {  // "static" function – no receiver
    return callCount
}

type Counter struct {
    Value int
}

func (c *Counter) Inc() {
    c.Value++
    callCount++
}

func main() {
    fmt.Println(GetCallCount())  // 0 – call without any Counter value
    c1 := &Counter{}
    c1.Inc()
    c1.Inc()
    fmt.Println(GetCallCount())  // 2
}
```

So: **“static” in Go = package-level `var` and `func` (no receiver).** Use the package name when calling from another package: **`mypkg.MyFunc()`**.

---

## 4. How to make an “abstract class” in Go

Go has **no** **abstract class** or **abstract** keyword. You cannot define a type that “must be extended” and never created by itself.

**What you do instead:** use an **interface**. The interface is the “abstract” part (only behavior, no data). Concrete types (structs + methods) are the “concrete classes” that “implement” that behavior.

**In Java/C# you might write:**
```text
abstract class Shape { abstract double Area(); }
class Circle extends Shape { ... }
```

**In Go:**

```go
package main

import (
    "fmt"
    "math"
)

// Interface = "abstract" contract (only method names, no implementation)
type Shape interface {
    Area() float64
}

// Concrete "class" 1
type Circle struct {
    Radius float64
}

func (c Circle) Area() float64 {
    return math.Pi * c.Radius * c.Radius
}

// Concrete "class" 2
type Rectangle struct {
    Width  float64
    Height float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

func main() {
    var s Shape
    s = Circle{Radius: 5}
    fmt.Println(s.Area())  // ~78.54
    s = Rectangle{Width: 4, Height: 3}
    fmt.Println(s.Area())  // 12
}
```

- **`Shape`** is an **interface** – it only says “anything that has **Area() float64** is a Shape.” No struct, no fields, no implementation. That’s the “abstract” part.
- **`Circle`** and **`Rectangle`** are concrete types. They both have **Area() float64**, so they **implement** **Shape** automatically (no **implements** keyword).
- You **cannot** create a value of type **Shape** by itself; you create **Circle** or **Rectangle** and assign them to a **Shape** variable.

So: **“abstract class” in Go = define an interface (behavior only), then define concrete structs with methods that match the interface.** See [14-interfaces.md](14-interfaces.md).

---

## 5. How to make an interface in Go

You **define an interface** by listing **method names** and their **signatures**. Any type that has those methods **implements** the interface automatically (no **implements** keyword).

**Syntax:**

```go
type InterfaceName interface {
    Method1(param type) returnType
    Method2(param type) returnType
}
```

**Example:**

```go
package main

import "fmt"

type Writer interface {
    Write(text string)
}

type ConsoleWriter struct{}

func (c ConsoleWriter) Write(text string) {
    fmt.Println("Writing:", text)
}

type FileWriter struct {
    Filename string
}

func (f FileWriter) Write(text string) {
    // pretend we write to file
    fmt.Println("Writing to file", f.Filename, ":", text)
}

func main() {
    var w Writer
    w = ConsoleWriter{}
    w.Write("Hello")  // Writing: Hello
    w = FileWriter{Filename: "out.txt"}
    w.Write("Hello")  // Writing to file out.txt : Hello
}
```

- **Writer** is the interface: “anything that has **Write(string)**.”
- **ConsoleWriter** and **FileWriter** both have **Write(string)**, so they both implement **Writer**.
- You can assign any of them to **`var w Writer`** and call **`w.Write(...)`**.

So: **to make an interface in Go: declare a type with `interface { Method1(...); Method2(...) }`. Any type that has those methods implements it.** Full details: [14-interfaces.md](14-interfaces.md).

---

## Quick comparison (Java/C# vs Go)

| Idea in Java/C#        | In Go |
|------------------------|--------|
| **class**              | **struct** + **methods** |
| **extends** (inheritance) | **embedding** (composition); no inheritance |
| **static** field/method | **package-level** **var** / **func** (no receiver) |
| **abstract class**     | **interface** + concrete **struct**s with methods |
| **interface**          | **interface** (same idea; no **implements** keyword) |

---

## Summary

1. **“Class”** → **struct** + **methods** ([11-structs.md](11-structs.md), [13-methods.md](13-methods.md)).
2. **“Extend parent”** → **embed** a struct (composition); same-name method = “override.”
3. **“Static”** → **package-level** **var** and **func** (no receiver).
4. **“Abstract class”** → **interface** (behavior only) + concrete structs that implement it.
5. **“Interface”** → **`type X interface { Method1(...); ... }`**; any type with those methods implements it ([14-interfaces.md](14-interfaces.md)).

**← [Back to INDEX](INDEX.md)** | Next: [14-interfaces.md](14-interfaces.md) for more on interfaces.
