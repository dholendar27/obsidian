---
date created: 2025-10-25 05:53
date updated: 2025-10-25 06:25
---

## Traits

The Traits are similar to interface in javascript or abstraction in python. It defines set of methods that types must implement.

#### Syntax

```rust
trait TraitName {
    // method signatures (no body)
    fn method_name(&self);

    // optional: methods with default implementation
    fn default_method(&self) {
        println!("This is a default implementation");
    }
}
```

#### Implementing Trait

```rust
trait Greet {
	fn say_hello(&self){
		println!("Hello");
	}
}
struct Person {
    name: String,
}

// Implement the Greet trait for Person
impl Greet for Person {
    fn say_hello(&self) {
        println!("Hello, my name is {}!", self.name);
    }
}

fn main() {
    let p = Person { name: String::from("Alice") };
    p.say_hello();
}

```

### Traits can also have **default methods**

```rust
trait Greet {
    fn greet(&self) {
        println!("Hello there!");
    }
}

struct Dog;

impl Greet for Dog {} // uses the default greet method

fn main() {
    Dog.greet(); // Output: Hello there!
}
```

## Generics

The Generics in rust allow us to write the code that will work for different datatypes without sacrificing performance and safety.
Generics are used to avoid the code duplication and keeps the code flexible and reusable.

#### Without Generics

```rust
fn largest_i32(list: &[i32]) -> i32 {
    let mut largest = list[0];
    for &item in list.iter() {
        if item > largest {
            largest = item;
        }
    }
    largest
}

fn largest_char(list: &[char]) -> char {
    let mut largest = list[0];
    for &item in list.iter() {
        if item > largest {
            largest = item;
        }
    }
    largest
}
```

The two function similar except the datatypes.

```rust
fn largest<T: PartialOrd + Copy>(list: &[T]) -> T {
    let mut largest = list[0];
    for &item in list.iter() {
        if item > largest {
            largest = item;
        }
    }
    largest
}
```

### Syntax of Generics

we can declare generics inside angle brackets (<>)

```rust
fn function_name<T>(param: T) {...}
```

Generics can appear in:

- Functions
- Structs
- Enums
- Methods
- Implementations

#### Structs

```rust
struct Point<T> {
    x: T,
    y: T,
}
```

This is mean that both `x` and `y` have the same type `T`.
we can use it as:

```rust
let int_point = Point { x: 5, y: 10 };
let float_point = Point { x: 1.0, y: 4.0 };
```

if we have different type, we can use multiple parameters.

```rust
struct Point<T, U> {
    x: T,
    y: U,
}

let mixed_point = Point { x: 5, y: 3.2 };
```

## Generics in Enums

Enums can also use generics. For example, the standard library defines `Option<T>` and `Result<T,E>` as:
```rust
enum Option<T> {
    Some(T),
    None,
}

enum Result<T,E> {
	Ok(T),
	Err(E),
}
```

## Trait Bounds - Restricting Generic type

Generics by themselves can represent any type, but sometimes we need to make sure that the type support certain operations.
```rust
fn print_largest<T: PartialOrd + std::fmt::Display>(x: T, y: T) {
    if x > y {
        println!("The largest is {}", x);
    } else {
        println!("The largest is {}", y);
    }
}
```

- `T: PartialOrd` → ensures `>` can be used.
- `T: Display` → ensures it can be printed.

```rust
fn print_largest<T, U>(x: T, y: T, msg: U)
where
    T: PartialOrd + Display,
    U: Display,
{
    if x > y {
        println!("{}: {}", msg, x);
    } else {
        println!("{}: {}", msg, y);
    }
}
```