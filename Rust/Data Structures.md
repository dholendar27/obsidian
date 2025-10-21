---
date created: 2025-10-21 08:09
date updated: 2025-10-21 08:13
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