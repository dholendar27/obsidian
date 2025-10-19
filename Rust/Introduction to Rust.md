---
date created: 2025-10-19 05:57
---

## What is Rust?

The Rust is a systems programming language that emphasises safety, speed and concurrency.

### Key features of Rust?

- **Memory Safety Without a Garbage Collector**: One of Rust’s core philosophies is ensuring memory safety while avoiding the overhead of a garbage collector (GC). It uses a system of _ownership_, _borrowing_, and _lifetimes_ to track memory usage and prevent issues like dangling pointers, buffer overflows, or use-after-free errors at compile time.
- **Zero-Cost Abstractions**: Rust provides high-level abstractions, like iterators or closures, but they don't introduce runtime overhead. Essentially, Rust tries to ensure that high-level constructs don't impact performance unless explicitly desired.
- **Concurrency Without Data Races**: Rust is designed to handle concurrency and parallelism safely. By using ownership and borrowing, the compiler ensures that no two threads can simultaneously access mutable data, avoiding data races.
- **Error Handling**: Rust promotes explicit error handling with its `Result` and `Option` types, forcing developers to handle potential errors at compile time rather than letting them slip by.
- **Performance**: Rust’s design is heavily optimized for performance. It’s often compared to C or C++ for system-level programming, but with a focus on safety.

## 2. Setting Up Rust (`rustup`, `cargo`, `rustc`)

To get started with Rust,we typically use the following tools:

#### **rustup**:

`rustup` is the recommended tool to install and manage Rust versions. It automatically sets up the Rust toolchain, and makes it easy to update or switch between different versions of Rust.

- **Installation**: You can install `rustup` by running:

```bash
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh  
```

- This installs the Rust toolchain (including `rustc`, `cargo`, and other tools).

#### **cargo**:

`cargo` is Rust’s package manager and build system. It handles dependencies, compiles code, and runs tests. It is an essential tool when working with Rust projects.

- **Basic commands**:
    - `cargo new <project-name>`: Creates a new Rust project with a basic directory structure.
    - `cargo build`: Builds the project.
    - `cargo run`: Builds and runs the project.
    - `cargo test`: Runs tests in your project.
    - `cargo update`: Updates dependencies in `Cargo.toml`.


#### **rustc**:

`rustc` is the Rust compiler. It’s typically invoked indirectly through `cargo`, but you can use it directly if needed for more control over the compilation process.

- Example: To compile a Rust file with `rustc` directly:
```bash
  rustc main.rs
  ./main  # Run the compiled program
```


### 3. Understanding the Compiler and `Cargo.toml`

#### **The Compiler**:

- **rustc** (Rust Compiler) reads your Rust source code, checks it for correctness, and generates machine code. It’s built to be extremely fast and efficient.
- Rust’s ownership model and strict type system ensure that many bugs (especially memory-related) are caught during compile time, before running the code.

#### **`Cargo.toml`**:

- The `Cargo.toml` file is where you configure your project’s dependencies, metadata, and build settings. It's like a `package.json` in JavaScript or a `pom.xml` in Java — it manages your project's settings and dependencies.
- Example `Cargo.toml`:

```toml
    [package]
    name = "my_project"
    version = "0.1.0"
    edition = "2021"
    
    [dependencies]
    serde = "1.0"
```

- `[package]`: Contains metadata like the name, version, and edition of your Rust project.
- `[dependencies]`: Lists the external libraries your project depends on (like `serde` in the example above).


Cargo handles downloading and building these dependencies when you run `cargo build` or other commands.

### **4. Basic Program Structure**

A basic Rust program consists of the following parts:

#### **The `main` function**:

Rust programs start by running the `main` function, just like C or C++ programs.

```rust
fn main() {
    println!("Hello, World!");
}
```

- `fn` is the keyword for defining a function in Rust.
- `println!` is a macro (denoted by `!`), which is used for printing to the console.

## Compilation in Rust
The Rust is a complied systems programming language, meaning code is turned into machine code before it runs.
### Rust Compilation process:
1. we write rust code in the rust file  `.rs`
2. If we are using the `rustc
	1. checks for syntax and type errors
	2. Enforces ownership/borrowing/lifetime rules
	3. Optimizes the code
	4. Converts it to machine code.
3. if we are using the Cargo, You usually use `cargo build` instead of running `rustc` directly. 
	1. Reads `Cargo.toml` for dependencies
	2. Builds your code and dependencies efficiently
	3. Caches builds so it’s faster the next time