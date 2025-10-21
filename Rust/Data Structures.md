---
date created: 2025-10-21 08:09
date updated: 2025-10-21 09:18
---

## Struct

The struct is an short for structure is an custom data type that lets us group related data together. struct are similar to objects in other programming language, but they don't have methods by default.

> we can define the methods in the struct using the `impl` blocks
> The Classic structs are also known as Named-field Structs

#### Basic Syntax

```rust
struct struct_name {
	item1: type,
	item2: type,
}
```

#### Example:

```rust
struct Person {
	name: String,
	age: u8,
	location: String,
}
```

### implement methods with `impl`

```rust
impl Person {
	fn greet(&self) {
		println!("Hello, my name is {}.", self.name)
	}
}
```

### Tuple structs

The Tuple structs behave like an tuple, but they have a name, making then a bit more meaningful.

```rust
struct Color(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    println!("Black color: {}, {}, {}", black.0, black.1, black.2);
}
```

- Fields are accessed with dot notation and a number and the index(e.g., `black.0`).
- Use case: when the meaning comes more from the struct name than from the field names.

## Enums

The Enums in the rust are a way to define a type that can be one of several different variants.

#### Syntax:

```rust
enum Enum_Name {
	type1,
	type2,
	type3,
}
```

#### Example

```rust
enum Direction {
    North,
    South,
    East,
    West,
}
```

This means: `Direction` can be **one of these 4 options**.
Enums are great when you want to:

- **Represent a fixed number of options** (e.g., days of the week, direction, traffic light color).
- **Group different kinds of data under one type** (more advanced, see below).

### Enums with data

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
}
```

Now, `Message` can be:

- `Quit` (no data),
- `Move` with `x` and `y` values,
- `Write` with a `String`.

This is like saying: _“I have different kinds of messages, and some carry data.”_

```rust
fn process_message(msg: Message) {
    match msg {
        Message::Quit => println!("Quit message"),
        Message::Move { x, y } => println!("Move to ({}, {})", x, y),
        Message::Write(text) => println!("Text message: {}", text),
    }
}
```

## What is Pattern Matching?

Pattern matching in Rust allows you to **destructure** complex data types and match their shapes and contents in a concise and readable way. It’s most commonly used with the `match` expression, but also appears in variable bindings, function parameters, and control flow constructs.

---

### 2. The `match` Statement

The `match` keyword compares a value against a series of patterns and executes the code block for the first pattern that matches.

#### Syntax:

```rust
match value {
    pattern1 => expr1,
    pattern2 => expr2,
    _ => default_expr,
}
```

#### Example:

```rust
let number = 3;

match number {
    1 => println!("One"),
    2 | 3 | 5 | 7 => println!("Prime number"),
    _ => println!("Something else"),
}
```

- `2 | 3 | 5 | 7` is a **pattern that matches any of those values**.

- `_` is the **wildcard pattern**, matching anything not previously matched.

---

### 3. Destructuring with Patterns

You can destructure tuples, structs, enums, and more directly in the match.

### Tuples:

```rust
let point = (0, 7);

match point {
    (0, y) => println!("On the y-axis at {}", y),
    (x, 0) => println!("On the x-axis at {}", x),
    (x, y) => println!("At point ({}, {})", x, y),
}
```

#### Structs:

```rust
struct Point { x: i32, y: i32 }

let p = Point { x: 1, y: 2 };

match p {
    Point { x: 0, y } => println!("On the y-axis at {}", y),
    Point { x, y: 0 } => println!("On the x-axis at {}", x),
    Point { x, y } => println!("At ({}, {})", x, y),
}
```

---

### 4. Enums and Pattern Matching

Rust enums are _ideal_ for pattern matching.

#### Example:

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}

let msg = Message::Move { x: 10, y: 20 };

match msg {
    Message::Quit => println!("Quit"),
    Message::Move { x, y } => println!("Move to ({}, {})", x, y),
    Message::Write(text) => println!("Write: {}", text),
    Message::ChangeColor(r, g, b) => println!("Color: {}, {}, {}", r, g, b),
}
```

---

### 5. Matching with Guards

You can add extra conditions using **`if` guards**.

```rust
let num = Some(4);

match num {
    Some(x) if x < 5 => println!("Less than five: {}", x),
    Some(x) => println!("Other number: {}", x),
    None => println!("No value"),
}
```

---

### 6. `@` Bindings

Use `@` to bind a value to a variable while also testing it against a pattern.

```rust
enum Message {
    Hello { id: i32 },
}

let msg = Message::Hello { id: 5 };

match msg {
    Message::Hello { id: id_variable @ 3..=7 } => {
        println!("Found id in range: {}", id_variable)
    }
    Message::Hello { id: 10..=12 } => println!("Found id in another range"),
    Message::Hello { id } => println!("Found other id: {}", id),
}
```

---

### 7. `if let` and `while let`

These are **concise** alternatives when you're only interested in one pattern.

#### `if let`:

```rust
let some_value = Some(3);

if let Some(x) = some_value {
    println!("Got: {}", x);
}
```

#### `while let`:

```rust
let mut stack = vec![1, 2, 3];

while let Some(top) = stack.pop() {
    println!("{}", top);
}
```

---

### 8. Nested Patterns

Patterns can be nested arbitrarily deep.

```rust
enum Color {
    Rgb(i32, i32, i32),
    Cmyk { c: i32, m: i32, y: i32, k: i32 },
}

let color = Color::Cmyk { c: 0, m: 128, y: 128, k: 0 };

match color {
    Color::Rgb(0, 0, 0) => println!("Black"),
    Color::Rgb(r, g, b) => println!("RGB({}, {}, {})", r, g, b),
    Color::Cmyk { c, m, y, k: 0 } => println!("CMY({},{},{})", c, m, y),
    _ => println!("Other color"),
}
```

---

### 9. Refutability

- **Refutable** patterns might not match (like in `if let`, `while let`).
- **Irrefutable** patterns always match (like in `let`, function args).

You can’t use refutable patterns in places where a match is guaranteed. For example:

```rust
let Some(x) = some_value; // ❌ compile error: pattern may not match
```

To fix this:

```rust
if let Some(x) = some_value { ... }
```

---

### 10. Pattern Matching Best Practices

- Always cover all cases (or use `_` for a catch-all).
- Be cautious with `match` on enums if new variants might be added in the future.
- Prefer `if let` for simpler, one-pattern matches to avoid unnecessary verbosity.
