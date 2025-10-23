
## 1. Foundations of Rust

1.1 [[Introduction to Rust]]  
 - [x] What is Rust and its philosophy  
 - [x] Setting up Rust (`rustup`, `cargo`, `rustc`)  
 - [x] Understanding the compiler and `Cargo.toml`  
 - [x] Basic program structure

1.2 [[Syntax and Core Language ]] 
 - [x] Variables and immutability  
 - [x] Data types and literals  
 - [x] Operators and expressions  
 - [x] [[Control flow]] (`if`, `match`, `loop`, `while`, `for`)

1.3 [[Memory Management]]
 - [x] Ownership rules  
 - [x] Borrowing and mutable references  
 • Lifetimes and annotations  
 - [x] Stack vs Heap  
 • Move and Copy semantics

1.4 [[Functions and Modules ]] 
 - [x] Defining functions and return values  
 - [x] Expressions vs statements  
 - [x] Modules and visibility (`mod`, `pub`, `use`)  
 • Crate structure and `super` keyword

1.5 [[Data Structures]]  
 - [x] Structs (tuple, unit, named)  
 - [x] Enums and pattern matching  
 • Arrays, tuples, and slices  
 • Strings (`String` vs `&str`)  
 • Collections (`Vec`, `HashMap`, `HashSet`, etc.)

---

## 2. Intermediate Rust

2.1 [[Error Handling]]  
 • `Result<T, E>` and `Option<T>`  
 • The `?` operator  
 • Custom error types  
 • Using `thiserror`, `anyhow`, `eyre`  
 • Recoverable vs unrecoverable errors

2.2 Traits and Generics  
 • Defining and implementing traits  
 • Trait bounds and generics  
 • Associated types and `where` clauses  
 • The `impl Trait` syntax

2.3 Memory Management  
 • Smart pointers (`Box<T>`, `Rc<T>`, `Arc<T>`)  
 • Interior mutability (`RefCell`, `Mutex`)  
 • Deref coercion and `Drop` trait  
 • Basics of `unsafe` usage

2.4 Iterators and Closures  
 • Closures and capturing environment  
 • Iterator trait and adaptors (`map`, `filter`, `fold`)  
 • Custom iterators and lazy evaluation

2.5 Testing and Documentation  
 • Unit and integration tests  
 • `#[test]` attribute and `cargo test`  
 • Documentation comments and `cargo doc`

---

## 3. Advanced Rust

3.1 Advanced Lifetimes  
 • Lifetime elision rules  
 • Static lifetimes  
 • Structs with references  
 • Complex lifetime relationships

3.2 Concurrency and Parallelism  
 • Threads and synchronization  
 • `Send` and `Sync` traits  
 • Channels, `Mutex`, and `RwLock`  
 • Async programming (`async`, `await`)  
 • Using `tokio` and `async-std`  
 • `Future` trait and combinators

3.3 Unsafe Rust  
 • What is `unsafe` and when to use it  
 • Raw pointers  
 • Manual memory management  
 • FFI (Foreign Function Interface)  
 • Unsafe traits and abstractions

3.4 Macros and Metaprogramming  
 • Declarative macros (`macro_rules!`)  
 • Procedural macros (`derive`, function-like, attribute)  
 • Working with `syn` and `quote`

3.5 Performance and Profiling  
 • Benchmarking (`cargo bench`)  
 • Profiling tools  
 • Inlining, LTO, and compiler optimizations  
 • Zero-cost abstractions  
 • Memory layout optimization

---

## 4. Rust Ecosystem and Tooling

4.1 Cargo and Build System  
 • Workspaces  
 • Custom build scripts (`build.rs`)  
 • Dependency management and versioning  
 • Feature flags and conditional compilation

4.2 Essential Crates  
 • `serde` – Serialization and deserialization  
 • `tokio` – Async runtime  
 • `reqwest` – HTTP client  
 • `clap` – CLI argument parsing  
 • `tracing` – Structured logging  
 • `diesel`, `sqlx` – Database interaction  
 • `actix`, `axum`, `rocket` – Web frameworks  
 • `tauri`, `bevy`, `dioxus` – Desktop/game/UI frameworks

4.3 Testing and CI/CD  
 • Advanced testing and mocking  
 • Property-based testing and fuzzing (`cargo-fuzz`)  
 • Continuous integration pipelines