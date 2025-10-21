---
date created: 2025-10-20 21:09
date updated: 2025-10-20 21:55
---

The functions are used to create an reusable blocks and are used to structure login, preform operations and return results

#### Basic Syntax:

```rust
fn function_name(parameter1: Type1, parameter2: Type2) -> ReturnType {

}
```

#### Example:

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

- `fn` is the keyword to define a function.
- `add` is the function name.
- `(a: i32, b: i32)` are parameters with types.
- `-> i32` indicates the return type.
- The expression `a + b` is the return value (no `return` keyword is required if it's the last expression).

### Key Features of Rust Functions

#### 1. Strong Typing

We must explicitly declare parameter types and return types.

```rust
fn square(x: i32) -> i32 {
    x * x
}
```

#### 2. Implicit Returns

Rust automatically returns the last expression if it doesn't end with a semicolon (`;`).

```rust
fn double(x: i32) -> i32 {
    x * 2  // Returned implicitly
}
```

But if you write:

```rust
x * 2;  // <- ends with semicolon, returns ()
```

#### 3. No Function Overloading

Unlike languages like C++ or Java, **Rust does not support function overloading** (i.e., multiple functions with the same name but different parameters).

#### 4. Function Pointers

You can pass functions as parameters to other functions using function pointers:

```rust
fn add_one(x: i32) -> i32 {
    x + 1
}

fn apply(f: fn(i32) -> i32, val: i32) -> i32 {
    f(val)
}

let result = apply(add_one, 5);  // result = 6
```

#### Return with `return` Keyword

You can use the `return` keyword for early exits or clarity:

```rust
fn maybe_add(a: i32, b: i32) -> i32 {
    if a > 10 {
        return a;
    }
    a + b
}
```

## Expression and Statement

### Expression

An Expression is anything that returns a value and does not include a semicolon at the end.

```rust
5               // returns 5
3 + 4           // returns 7
"hello"         // returns &str
some_function() // returns whatever the function returns
```

You can assign expressions to variables:

```rust
let x = 5 + 3; // '5 + 3' is an expression that evaluates to 8
```

Blocks can also be expressions:

```rust
let y = {
    let a = 10;
    let b = 20;
    a + b       // This is an expression; result is 30
};
```

### Statements

A Statement is an instruction that performs an action but does not return a value.

```rust
let x = 5;     // variable declaration is a statement
fn foo() {}    // function definition is a statement
```

Statements **end with a semicolon**.

If you put a semicolon at the end of an expression, it becomes a statement:

```rust
5 + 3; // This is a statement now. The value is discarded.
```

## Modules

- Modules in Rust help in splitting a program into logical units for better readability and organization.
- Once a program gets larger, it is important to split it into multiple files or ==namespaces==. Modules help in structuring our program.
- This means they **cannot** be accessed from outside the module unless you specifically make them **public** by adding the `pub` keyword.
### Defining a Module in Rust

The `mod` keyword is used to define a module. The syntax of module is:
```rust
// syntax of a module
mod module_name {
  // code
}
```

#### Example:
```rust
mod math {
    // This function is part of the 'math' module
    pub fn add(a: i32, b: i32) -> i32 {
        a + b
    }

    pub fn subtract(a: i32, b: i32) -> i32 {
        a - b
    }
}

fn main() {
    let result = math::add(5, 3); // Using the 'add' function from 'math' module
    println!("The result is: {}", result);
}
```

- `mod math` creates a module named `math`.
- The functions `add` and `subtract` are part of this module.
- You can access these functions using `math::add` and `math::subtract` in the `main` function.
#### Visibility with `pub`:

By default, everything in a module is **private** to the module. If you want to use a function, struct, or other item from outside the module, you need to make it **public** by adding the `pub` keyword in front of it.

For example, in the `math` module, the functions `add` and `subtract` are marked `pub`, meaning they can be accessed from outside the module.

```rust
mod math {
    fn add(a: i32, b: i32) -> i32 {
        a + b
    }
}
```

You **cannot** call `math::add` outside of the module, because it's private.

### Nested Modules:

Modules can also be nested inside other modules, just like folders inside folders. You create nested modules using `mod` again. Here's an example:
```rust
mod math {
    pub mod geometry {
        pub fn area_of_circle(radius: f64) -> f64 {
            std::f64::consts::PI * radius * radius
        }
    }

    pub fn add(a: i32, b: i32) -> i32 {
        a + b
    }
}

fn main() {
    let circle_area = math::geometry::area_of_circle(5.0);
    println!("The area of the circle is: {}", circle_area);
}
```

### File-Based Module System in Rust

Rust allows you to create modules in separate files, which can then be referenced and used in your main code. Let's break it down step by step with an example.

---

#### Step 1: Project Structure

Consider a Rust project with the following structure:

```
src/
├── main.rs
├── calculator.rs
└── geometry.rs
```

Here:

- `main.rs` will be the entry point of the program (the main file).
- `calculator.rs` and `geometry.rs` will contain the logic for different modules.

---

#### Step 2: Creating the Modules in Separate Files

##### `calculator.rs` (Module file for `calculator`):

```rust
// calculator.rs
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

pub fn subtract(a: i32, b: i32) -> i32 {
    a - b
}
```

In this file, we define a **module** called `calculator`. The functions `add` and `subtract` are marked with `pub`, making them **public** and accessible from outside this module.

##### `geometry.rs` (Module file for `geometry`):

```rust
// geometry.rs
pub fn area_of_circle(radius: f64) -> f64 {
    std::f64::consts::PI * radius * radius
}

pub fn area_of_rectangle(length: f64, width: f64) -> f64 {
    length * width
}
```

Here, we define a **module** called `geometry`, which contains functions to calculate areas. Again, the functions are marked with `pub` to make them accessible outside this module.

---

#### Step 3: Linking the Modules in `main.rs`

Now that we have separate files for each module (`calculator.rs` and `geometry.rs`), we need to **link** them to `main.rs` so we can use the functions defined in them.

##### `main.rs` (The entry point of the application):

```rust
// main.rs
mod calculator; // Declare the 'calculator' module
mod geometry;   // Declare the 'geometry' module

fn main() {
    // Using the 'add' function from 'calculator' module
    let sum = calculator::add(10, 5);
    println!("Sum: {}", sum);

    // Using the 'area_of_circle' function from 'geometry' module
    let area = geometry::area_of_circle(5.0);
    println!("Area of circle: {}", area);

    // Using the 'area_of_rectangle' function from 'geometry' module
    let rectangle_area = geometry::area_of_rectangle(10.0, 5.0);
    println!("Area of rectangle: {}", rectangle_area);
}
```

In `main.rs`:

- We use the `mod` keyword to declare the modules (`calculator` and `geometry`).
- After declaring the modules, we can use the functions from these modules like `calculator::add` and `geometry::area_of_circle`.
#### How Does It Work?

1. The `mod calculator;` and `mod geometry;` lines tell Rust to look for files named `calculator.rs` and `geometry.rs` in the same directory as `main.rs` and treat them as modules.
2. The `pub` keyword in `calculator.rs` and `geometry.rs` makes the functions available for use outside of their respective modules.
3. In `main.rs`, we access the functions by prefixing them with the module name (`calculator::add` or `geometry::area_of_circle`).

---

#### Step 4: Organizing Large Modules with Submodules

As your project grows, you might want to organize large modules into **submodules**. You can do this by creating directories and adding `mod.rs` files inside them.
For example, let's say you have a `shapes` module with multiple submodules for different shapes:

```
src/
├── main.rs
└── shapes/
    ├── mod.rs
    ├── circle.rs
    └── rectangle.rs
```

#### `shapes/mod.rs`:

```rust
// This is the root module for 'shapes'

// Declare submodules
pub mod circle;
pub mod rectangle;
```

#### `shapes/circle.rs`:

```rust
// circle.rs
pub fn area(radius: f64) -> f64 {
    std::f64::consts::PI * radius * radius
}
```

#### `shapes/rectangle.rs`:

```rust
// rectangle.rs
pub fn area(length: f64, width: f64) -> f64 {
    length * width
}
```

#### `main.rs`:

```rust
// main.rs
mod shapes; // Declare the 'shapes' module

fn main() {
    let circle_area = shapes::circle::area(5.0);
    println!("Area of circle: {}", circle_area);

    let rectangle_area = shapes::rectangle::area(10.0, 5.0);
    println!("Area of rectangle: {}", rectangle_area);
}
```

### How It Works:

- The `shapes/mod.rs` file acts as the entry point for the `shapes` module. It declares the submodules `circle` and `rectangle` as public.
- In `main.rs`, you can now access the functions from both `shapes::circle` and `shapes::rectangle`.

## `use`
In Rust, the `use` keyword is used to bring items (functions, structs, modules, etc.) into scope so that you can use them without having to refer to their full path each time. It’s similar to `import` in other languages like Python or JavaScript.

### Why Use `use`?

Without `use`, you'd have to reference every item with its full path, which can be cumbersome. `use` allows you to shorten the path, making your code cleaner and more readable.

```rust
use module_name::item_name;
```