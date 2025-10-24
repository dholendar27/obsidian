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

> [!important]+ Important
> 
> - The keys **are owned by the `HashMap`** (moved into it when you insert).
> - When you want to look something up, instead of moving (giving ownership of) the key **again**, you just **borrow** it with a reference (`&key`).
> - This lets the `HashMap` look up the value _without taking ownership_ of the key you’re using to query.


---

### 3. Iterating

You can iterate over keys and values:

```rust
for (key, value) in &scores {
    println!("{}: {}", key, value);
}
```

---

## Updating a HashMap

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

// println!("{}", name); Error! name was moved into the HashMap
```

- When you insert an owned type (like `String`), ownership moves.
- If you need to keep using the variable, insert a **reference** instead.

---

### Borrowing Keys When Accessing

When you do `map.get(&key)`, you borrow the key (`&K`), not move it.
The same goes for iteration (`for (k, v) in &map`).

---

## Performance and Capacity

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

## HashMap vs BTreeMap

|Feature|HashMap|BTreeMap|
|---|---|---|
|Ordering|Unordered|Sorted by key|
|Performance|O(1) average|O(log n)|
|Memory locality|Worse|Better|
|Hashing|Yes|No|

If you need sorted keys, use `BTreeMap`.

---

## Common Methods

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

## Vectors

### What is a Vector in Rust?

A **vector** (`Vec<T>`) is a **growable, heap-allocated** collection of elements of the same type `T`. Unlike arrays, whose size is fixed at compile time, vectors can grow or shrink at runtime.

### Key characteristics:

- Stores elements contiguously in memory.
- Allows random access to elements by index.
- Supports push/pop operations.
- Uses heap allocation to store elements, allowing dynamic size.

---

### How to Create a Vector

#### 1. Using `Vec::new()`

```rust
let mut v: Vec<i32> = Vec::new();
v.push(1);
v.push(2);
v.push(3);
```

You need `mut` because you want to mutate the vector by pushing elements.

#### 2. Using the `vec!` Macro

```rust
let v = vec![1, 2, 3, 4];
```

This creates a vector initialized with those values.

---

### Accessing Elements

You can access elements by index:

```rust
let third = v[2]; // indexing starts at 0
println!("The third element is {}", third);
```

But be careful! Indexing out of bounds will cause a **panic at runtime**.

Alternatively, use `.get()` which returns an `Option`:

```rust
match v.get(2) {
    Some(third) => println!("Third element is {}", third),
    None => println!("There is no third element."),
}
```

---

### Modifying Vectors

#### Adding Elements

- `.push(element)` — adds element to the end.
    
- `.insert(index, element)` — inserts element at a specific index.
    

Example:

```rust
v.push(5);
v.insert(1, 10);
```

#### Removing Elements

- `.pop()` — removes and returns the last element (returns `Option<T>`).
    
- `.remove(index)` — removes element at index and shifts others.
    

Example:

```rust
let last = v.pop();
let second = v.remove(1);
```

---

### Iterating Over Vectors

You can iterate over vectors easily:

```rust
for i in &v {
    println!("{}", i);
}
```

- `&v` iterates over references to elements (`&T`).
- `v.iter_mut()` gives mutable references for modification.
    

---

### Memory and Performance

- Vectors allocate memory on the heap.
- They manage capacity internally — they allocate extra space to avoid reallocating on every push.
- When capacity is exceeded, the vector allocates a larger buffer and copies elements over.
- You can check or set capacity:

```rust
println!("Capacity: {}", v.capacity());
v.reserve(10); // pre-allocate space for 10 more elements
```

---

## Iterator
### What Is an Iterator?

An **iterator** in Rust is **any type that implements the `Iterator` trait**:

```rust
pub trait Iterator {
    type Item;

    fn next(&mut self) -> Option<Self::Item>;
}
```

- Each call to `next()` returns an `Option<Item>`:
    - `Some(item)` for the next element
    - `None` when the iteration is finished

You rarely call `next()` directly — instead, you use methods like `.map()`, `.filter()`, `.collect()`, etc.

---

### `iter()`, `iter_mut()`, and `into_iter()`

These three methods determine **how you iterate** over a collection (especially slices or vectors).

Let’s take an example:

```rust
let v = vec![1, 2, 3];
```

#### 1. `iter()` → Immutable references

```rust
for x in v.iter() {
    println!("{}", x); // x: &i32
}
```

- **Type**: `Iter<'_, T>` (iterator over `&T`)
- **Does not consume** the collection.
- You can still use `v` after the loop.

 Use when you just need to **read** elements.

---

#### 2. `iter_mut()` → Mutable references

```rust
let mut v = vec![1, 2, 3];
for x in v.iter_mut() {
    *x += 1;
}
println!("{:?}", v); // [2, 3, 4]
```

- **Type**: `IterMut<'_, T>` (iterator over `&mut T`)
- Allows you to **mutate** the elements in place.
- Does **not consume** the collection.

 Use when you need to **modify** elements.

---

#### 3. `into_iter()` → Takes ownership

```rust
let v = vec![1, 2, 3];
for x in v.into_iter() {
    println!("{}", x); // x: i32
}
```

- **Type**: `IntoIter<T>` (iterator over `T`)
- **Consumes** the collection — `v` is moved and can’t be used after.
- Ownership of each element is transferred into the loop.

 Use when you want to **consume** the collection or move elements out.

---

###  Special case for arrays

For arrays (not vectors), `into_iter()` used to behave differently before Rust 1.53 — it now works consistently:

```rust
let arr = [1, 2, 3];
for x in arr.into_iter() {
    println!("{}", x);
}
```

This moves the array elements, similar to a vector.

---

##  Consuming vs Non-Consuming Adapters

Iterators can be **chained** using _adapters_ — methods that transform or consume them.

---

### 1. **Non-consuming adapters**

These **produce a new iterator**, leaving the original unconsumed (until you actually iterate).

Examples:

```rust
let v = vec![1, 2, 3];
let iter = v.iter()
    .map(|x| x * 2)
    .filter(|x| *x > 2);
```

- `map`, `filter`, `take`, `skip`, etc.
    
- They **return a new iterator**, and no data is processed until you call something that **consumes** it.
    

---

### 2. **Consuming adapters**

These **consume** the iterator to produce a value.

Examples:

```rust
let v = vec![1, 2, 3];

let sum: i32 = v.iter().sum();         // Consumes the iterator
let collected: Vec<_> = v.iter().collect(); // Collects into a Vec
```

Other examples: `.count()`, `.find()`, `.all()`, `.any()`, `.fold()`, `.for_each()`, etc.
