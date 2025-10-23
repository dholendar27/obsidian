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
---
## Error Handling
### What is Error Handling?

When you write a program, sometimes things go wrong — like trying to open a file that doesn’t exist, or dividing by zero. Error handling is how your program deals with these problems without crashing.

---

### How Does Rust Handle Errors?

Rust has a **safe and clear way** to handle errors. It mainly uses **two types** of errors:

1. **Recoverable errors** — errors that your program can fix or handle, like “file not found.”
2. **Unrecoverable errors** — serious problems that usually mean your program must stop, like bugs.
    

---

#### 1. Recoverable Errors — The `Result` Type

Rust uses a special type called `Result` to handle recoverable errors.

- `Result<T, E>` is an **enum** (a kind of special type with two possibilities):
    - `Ok(T)` — means success, with a value of type `T`.
    - `Err(E)` — means failure, with an error of type `E`.

Example:

```rust
fn divide(a: i32, b: i32) -> Result<i32, String> {
    if b == 0 {
        Err(String::from("Cannot divide by zero"))
    } else {
        Ok(a / b)
    }
}
```

Here, the function returns:

- `Ok(result)` if division works,
- `Err("Cannot divide by zero")` if `b` is zero.

You **handle** the `Result` by checking if it’s `Ok` or `Err` using:
- `match` expressions
- Helper methods like `.unwrap()`, `.expect()`, `.unwrap_or()`, `.map()`, `.and_then()`, etc.

Example of using `match`:

```rust
match divide(10, 2) {
    Ok(value) => println!("Result is {}", value),
    Err(e) => println!("Error: {}", e),
}
```

---

#### 2. Unrecoverable Errors — The `panic!` Macro

Sometimes, something is so wrong that the program can’t continue safely. Rust uses `panic!` to stop execution immediately.

Example:

```rust
panic!("Something went terribly wrong!");
```

When your program panics, it prints an error message and exits.

---

#### Why is Rust’s Error Handling Special?

- Rust **forces** you to handle errors explicitly. You can’t ignore them silently.
- This makes your programs **more reliable** and **safe**.
- It prevents many bugs that happen when errors are ignored.

---
