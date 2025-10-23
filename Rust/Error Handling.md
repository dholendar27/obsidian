---
date created: 2025-10-23 14:48
date updated: 2025-10-23 14:57
---

## Option

In Rust, `Option` is an enum (a type that can be one of several variants) that represent the possibility of having a value or not.
It is defined in the standard library as:

```rust
enum Option<T> {
	Some(T),
	None,
}
```

- `T` is a **generic type parameter** — meaning `Option<T>` can hold _any_ type (`Option<i32>`, `Option<String>`, `Option<MyStruct>`, etc.).
- `Some(T)` means “we have a value.”
- `None` means “no value.”

### Why Rust Has `Option` instead of `null`

In many languages, a variable can be `None` or `null`, and if we forgot to check before using it, program can crash (e.g., `NullPointerException` )

Rust has **no null values**. Instead, it uses `Option` to make the possibility of missing data **explicit and safe** at compile time.
That means:

- If a value _might_ be missing, it must be wrapped in an `Option`.
- The compiler **forces you** to handle both `Some` and `None` cases before using the value.

This removes an entire class of bugs related to null references.

#### Example

```rust
fn divide(a: f64, b: f64) -> Option<f64> {
    if b == 0.0 {
        None  // division by zero: no result
    } else {
        Some(a / b)
    }
}

fn main() {
    let result = divide(10.0, 2.0);

    match result {
        Some(value) => println!("Result: {}", value),
        None => println!("Cannot divide by zero!"),
    }
}
```

- If the division is valid, we return `Some(result)`.
- If invalid, we return `None`.
- The `match` statement forces us to handle _both cases_ — no chance of runtime panic due to a missing value.
