# Interfaces in Go

**← [Back to INDEX](INDEX.md)**

An **interface** is like a **contract**: it only says *what* methods a type must have (names and shapes), not *how* they work. If your type has all those methods, it "implements" the interface. You can then use that type wherever the interface is expected. Everything below is in simple language.

**Concepts used in this page:** We use **structs** ([11-structs.md](11-structs.md)) and **methods** ([13-methods.md](13-methods.md)). Read those first if you haven’t.

---

## Why use interfaces?

- **Reuse code** – write functions that work with “anything that can do X” (e.g. anything that can `Read` and `Write`).
- **Test easily** – you can pass a fake or mock type that implements the same interface.
- **Decouple** – code depends on behavior (methods), not on one specific type.

---

## Defining an interface

You list **method names** and their **signatures** – that means the name of the method plus its parameter types and return types. You don’t write the code here; the types that **implement** the interface (i.e. have those methods) provide the code.

```go
type Writer interface {
    Write(data []byte) (int, error)
}
```

So “**Writer** is anything that has a **Write** method with this signature.”

---

## A type implements an interface automatically

In Go you **don’t** say “type X implements interface Y.” If a type has **all the methods** of an interface, it **implements** that interface. No extra keyword.

```go
package main

import "fmt"

type Greeter interface {
    Greet() string
}

type Person struct {
    Name string
}

func (p Person) Greet() string {
    return "Hello, I am " + p.Name
}

func main() {
    var g Greeter = Person{Name: "Alice"}
    fmt.Println(g.Greet())  // Hello, I am Alice
}
```

**Person** has a **Greet() string** method, so **Person** implements **Greeter**. We can assign **Person** to a variable of type **Greeter**.

---

## Interface variable holds a value

A variable of interface type (e.g. **Greeter**) can hold **any value** whose type implements that interface. You can then call the interface’s methods on that value.

```go
var g Greeter
g = Person{Name: "Bob"}
fmt.Println(g.Greet())  // Hello, I am Bob
```

---

## Empty interface `any`

**`any`** (or **`interface{}`** in older code) is an interface with **no methods**. Every type satisfies it. So a variable of type **`any`** can hold **any** value.

```go
var x any
x = 42
x = "hello"
x = []int{1, 2, 3}
```

Use **`any`** when you need to accept or store “anything.” You often need a **type assertion** later to get the concrete type back.

---

## Type assertion

If you have an interface value and you believe it holds a specific type, you can **assert** that type:

```go
var g Greeter = Person{Name: "Alice"}
p, ok := g.(Person)  // safe: ok is true if g holds a Person
if ok {
    fmt.Println(p.Name)  // Alice
}
```

**`value.(Type)`** – if the value is of that type, you get the value and **ok** is true. If not, you get the zero value and **ok** is false (no panic if you use the two-result form).

---

## Type switch

A **type switch** checks the **dynamic type** of an interface value. You write **`switch v := x.(type)`** and then **`case Type:`** for each type you care about.

```go
package main

import "fmt"

func describe(x any) {
    switch v := x.(type) {
    case int:
        fmt.Println("int:", v)
    case string:
        fmt.Println("string:", v)
    case bool:
        fmt.Println("bool:", v)
    default:
        fmt.Printf("other type: %T, value: %v\n", v, v)
    }
}

func main() {
    describe(42)       // int: 42
    describe("hello")  // string: hello
    describe(true)     // bool: true
    describe(3.14)     // other type: float64, value: 3.14
}
```

Inside each **case**, **`v`** has that **concrete type**, so you can use it without another assertion. Use **`default`** for any other type.

---

## Summary

| Idea | Example |
|------|---------|
| Define interface | `type Writer interface { Write([]byte) (int, error) }` |
| Implement | Give a type those methods (no keyword) |
| Use interface | `var w Writer = myType{}` |
| Any type | `any` or `interface{}` |
| Type assertion | `v, ok := x.(ConcreteType)` |
| Type switch | `switch v := x.(type) { case int: ... case string: ... default: ... }` |

Interfaces let you write code that depends on **behavior** (methods), not on one concrete type.

**Coming from Java/C#?** For class, extends, static, abstract class, and interface in one place, see [24-oop-in-go.md](24-oop-in-go.md).

**← [Back to INDEX](INDEX.md)** | Next: [15-packages.md](15-packages.md) – **Packages**: organize code and use other code.
