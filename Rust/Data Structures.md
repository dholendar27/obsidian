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

---

## What Is a HashMap in Rust?

A **`HashMap`** in Rust is a collection type that stores **key-value pairs**, where each _key_ must be unique, and each _value_ is associated with one key.

It is part of Rust’s standard library:

```rust
use std::collections::HashMap;
```

Conceptually, it’s similar to:

- **Python’s `dict`**
- **Java’s `HashMap`**
- **C++’s `unordered_map`**

---

## Internal Mechanics

At its core, a `HashMap` uses a **hash function** to map keys to _buckets_ in memory. When you insert or access a key, Rust:

1. Computes the hash of the key.
2. Finds the corresponding bucket.
3. Stores or retrieves the key-value pair.

Rust’s `HashMap` uses **SipHash**, a cryptographically strong hash function designed to prevent _hash flooding attacks_ (i.e., denial-of-service via collisions).

---

## Basic Usage

### 1. Creating a HashMap

```rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Red"), 50);

    println!("{:?}", scores);
}
```

- `HashMap::new()` creates an empty map.
- `.insert(key, value)` adds entries.
- The keys and values must have the **same type** throughout the map.
> [!important]+ Important
> A `HashMap<K, V>` **stores keys and values by ownership** — meaning:
>- It owns its keys and values internally.
>- When you insert a key, you give up ownership of that key.


---

### 2. Accessing Values

```rust
let team_name = String::from("Blue");
let score = scores.get(&team_name);

match score {
    Some(value) => println!("Score: {}", value),
    None => println!("No score found"),
}
```

- `.get(&key)` returns an `Option<&V>` (`Some(&value)` or `None`).
    
- You must pass a _reference_ to the key.
    

You can also use `unwrap_or` to simplify:

```rust
let score = scores.get(&team_name).unwrap_or(&0);
```

---

### 3. Iterating

You can iterate over keys and values:

```rust
for (key, value) in &scores {
    println!("{}: {}", key, value);
}
```

---

## 🧱 Updating a HashMap

### 1. Overwriting a Value

If you insert a key that already exists, the old value is replaced:

```rust
scores.insert(String::from("Blue"), 25);
```

Now "Blue" has a new value.

---

### 2. Inserting Only If Absent

`.entry()` + `.or_insert()` is a powerful pattern:

```rust
scores.entry(String::from("Yellow")).or_insert(30);
scores.entry(String::from("Blue")).or_insert(50);
```

- If the key doesn’t exist, insert the provided value.
    
- If it exists, do nothing.
    
- Returns a mutable reference to the value.
    

---

### 3. Updating Based on the Old Value

You can modify existing values directly through `.entry()`:

```rust
let text = "hello world wonderful world";
let mut map = HashMap::new();

for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0);
    *count += 1;
}
println!("{:?}", map);
```

This counts word frequency — a common idiom in Rust.

---

##  Ownership and Borrowing Rules

This is where Rust’s memory model shines (and can trip up newcomers).

### Keys and Values Are Moved Into the Map

```rust
let name = String::from("Alice");
let mut map = HashMap::new();
map.insert(name, 10);

// println!("{}", name); ❌ Error! name was moved into the HashMap
```

- When you insert an owned type (like `String`), ownership moves.
    
- If you need to keep using the variable, insert a **reference** instead.
    

---

### Borrowing Keys When Accessing

When you do `map.get(&key)`, you borrow the key (`&K`), not move it.

The same goes for iteration (`for (k, v) in &map`).

---

## ⚡ Performance and Capacity

### 1. Default Hasher

Rust’s standard `HashMap` uses **SipHash** for security (against collision attacks).  
However, it’s not the fastest. For performance-critical code, use **`hashbrown`** or **`fxhash`**:

```toml
# In Cargo.toml
[dependencies]
hashbrown = "0.14"
```

Then:

```rust
use hashbrown::HashMap;
```

### 2. Reserving Capacity

If you know how many elements you’ll insert, preallocate:

```rust
let mut map = HashMap::with_capacity(100);
```

This avoids reallocations.

---

## 🔒 HashMap vs BTreeMap

|Feature|HashMap|BTreeMap|
|---|---|---|
|Ordering|Unordered|Sorted by key|
|Performance|O(1) average|O(log n)|
|Memory locality|Worse|Better|
|Hashing|Yes|No|

If you need sorted keys, use `BTreeMap`.

---

## 🧰 Common Methods

|Method|Description|
|---|---|
|`insert(key, val)`|Adds or updates a key-value pair|
|`get(&key)`|Retrieves a value (Option<&V>)|
|`remove(&key)`|Removes an entry|
|`contains_key(&key)`|Checks for existence|
|`keys()` / `values()`|Iterators over keys/values|
|`entry(key)`|Access or insert default value|
|`clear()`|Empties the map|

---

## 🧩 Example: Frequency Counter

```rust
use std::collections::HashMap;

fn main() {
    let text = "hello world wonderful world";
    let mut word_count = HashMap::new();

    for word in text.split_whitespace() {
        *word_count.entry(word).or_insert(0) += 1;
    }

    for (word, count) in &word_count {
        println!("{}: {}", word, count);
    }
}
```

Output:

```
hello: 1
world: 2
wonderful: 1
```

---

## 🧪 Example: Custom Struct as Key

If you use custom types as keys, they must implement:

- `Eq`
    
- `Hash`
    

Example:

```rust
use std::collections::HashMap;

#[derive(Hash, Eq, PartialEq, Debug)]
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let mut map = HashMap::new();
    map.insert(Point { x: 1, y: 2 }, "A");
    map.insert(Point { x: 3, y: 4 }, "B");

    println!("{:?}", map.get(&Point { x: 1, y: 2 }));
}
```

---

## 🚨 Common Pitfalls

1. **Forgetting Ownership Transfer**
    
    - `String` and `Vec` get moved into the map.
        
2. **Relying on Order**
    
    - HashMaps are **unordered** — iteration order is not stable.
        
3. **Hash Collisions**
    
    - Rare, but can affect performance (mitigated by SipHash).
        

---

## 🧩 Summary

|Concept|Description|
|---|---|
|Collection Type|Key–Value pairs|
|Complexity|Average O(1) lookup/insert|
|Keys|Must implement `Eq` + `Hash`|
|Values|Any type|
|Ordering|Unordered|
|Safe Concurrency|Use `std::sync::Mutex<HashMap<…>>` or `dashmap`|
