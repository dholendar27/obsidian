---
date created: 2025-10-19 20:27
---

## 1. `if` Statements

Rust’s `if` is similar to other languages but with some twists.

### Syntax:

```rust
if condition {
    // code if condition is true
} else if another_condition {
    // code if another_condition is true
} else {
    // code if none above is true
}
```

### Key points:

- The condition **must** be a boolean expression (`bool`).
- `if` is an expression, meaning it returns a value.
- Because it returns a value, you can assign its result to a variable.

### Example:

```rust
let number = 7;
let result = if number % 2 == 0 {
    "even"
} else {
    "odd"
};

println!("The number is {}", result);  // Output: The number is odd
```

---

## 2. `match` Expression
`match` is powerful pattern matching and exhaustive checking.
### Syntax:

```rust
match value {
    pattern1 => expr1,
    pattern2 => expr2,
    _ => default_expr,
}
```

- `_` is a wildcard that matches anything.
- Every possible pattern **must** be covered (exhaustiveness).
- `match` is also an expression, so it returns a value.

### Example:

```rust
let number = 3;

let result = match number {
    1 => "one",
    2 => "two",
    3 => "three",
    _ => "something else",
};

println!("Number is {}", result); // Output: Number is three
```

### More complex pattern matching:

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
}

let msg = Message::Move { x: 10, y: 15 };

match msg {
    Message::Quit => println!("Quit"),
    Message::Move { x, y } => println!("Move to {}, {}", x, y),
    Message::Write(text) => println!("Text message: {}", text),
}
```

---

## 3. Loops in Rust

Rust has three main loop types:

### 3.1 `loop`

- An infinite loop unless you `break`.
- Returns a value via `break` with a value.

```rust
let mut count = 0;

let result = loop {
    count += 1;

    if count == 5 {
        break count * 2;  // break with a value
    }
};

println!("Result is {}", result);  // Output: Result is 10
```

### 3.2 `while`

- Loop while condition is true.

```rust
let mut number = 3;

while number != 0 {
    println!("{}!", number);
    number -= 1;
}

println!("LIFTOFF!");
```

### 3.3 `for`

- Iterate over elements of an iterator or collection.

```rust
let a = [10, 20, 30, 40, 50];

for element in a {
    println!("the value is: {}", element);
}
```

- Also used for ranges:

```rust
for number in 1..5 {
    println!("{}", number); // prints 1, 2, 3, 4 (end exclusive)
}

for number in 1..=5 {
    println!("{}", number); // prints 1, 2, 3, 4, 5 (end inclusive)
}
```

---

## Summary Table

| Control Flow | Description                 | Expression/Statement          | Example Return Value |
| ------------ | --------------------------- | ----------------------------- | -------------------- |
| `if`         | Conditional branching       | Expression (returns value)    | Yes                  |
| `match`      | Pattern matching            | Expression (returns value)    | Yes                  |
| `loop`       | Infinite loop until `break` | Expression (can return value) | Yes (via `break`)    |
| `while`      | Loop while condition true   | Statement (no return value)   | No                   |
| `for`        | Iterate over iterator       | Statement (no return value)   | No                   |
