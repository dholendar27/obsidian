---
date created: 2025-10-25 05:53
date updated: 2025-10-25 06:16
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