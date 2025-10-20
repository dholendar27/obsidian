---
date created: 2025-10-20 12:42
date updated: 2025-10-20 16:38
---

### 1. **Garbage Collected (GC) Languages**

**Examples:** Java, C#, Python, Go, JavaScript

#### How it works:

- The programmer allocates memory (implicitly or explicitly) when creating objects.
- A Garbage Collector (GC) periodically scans memory to find objects no longer reachable (no references pointing to them).
- It automatically frees memory for these unreachable objects.

#### Types of garbage collection:

- **Mark and Sweep:** Marks all reachable objects, then sweeps through and deletes unmarked ones.

- **Reference Counting:** Objects are deleted when no references to them exist.

- **Generational GC:** Optimizes GC by treating objects of different ages differently (e.g., Java’s HotSpot GC).

#### Pros:

- Easy to use: No need to manually manage memory.
- Less error-prone: Avoids common bugs like double free or dangling pointers.

#### Cons:

- Performance overhead: GC pauses can cause latency.
- Unpredictable timing: Memory isn’t freed immediately, which may cause spikes in memory use.

---

### 2. **Manual Memory Management**

**Examples:** C, C++

#### How it works:

- The programmer manually allocates memory (e.g., with `malloc`, `new`) and manually frees it (`free`, `delete`).
- The system doesn’t help you; if you forget to free memory, it leaks.

#### Pros:

- Full control over memory and performance.
- No runtime GC overhead.

#### Cons:

- Error-prone:
  - Memory leaks (forgetting to `free`)
  - Double-free errors
  - Dangling pointers (freeing memory still in use)
- Hard to maintain in large codebases.

---

### 3. **The Rust Way (Ownership Model)**

**Language:** Rust

Rust uses a unique approach called ownership-based memory management, combining performance similar to manual memory management with safety similar to garbage-collected languages, without using a garbage collector.

#### How it works:

Rust has three main rules in its ownership model:

1. Each value has a single owner.
2. When the owner goes out of scope, the value is automatically dropped (freed).
3. You can borrow references to values, but with strict rules:
   - One mutable reference OR
   - Any number of immutable references (but not both at once)

#### Example:

```rust
fn main() {
    let s = String::from("hello"); // s owns the String
    takes_ownership(s);           // s is moved here, ownership transferred
    // s can’t be used here anymore

    let x = 5;
    makes_copy(x);                // i32 is Copy, so x can still be used
}

fn takes_ownership(some_string: String) {
    println!("{}", some_string);
}

fn makes_copy(some_integer: i32) {
    println!("{}", some_integer);
}
```

#### Pros:

- No garbage collector = predictable performance.
- Memory safety without manual allocation/deallocation.
- Compile-time checks prevent many runtime bugs.

#### Cons:

- Steep learning curve due to ownership/borrowing rules.
- Complex lifetimes can be tricky in advanced cases.

## Memory Management in Rust

Rust achieve the memory management using the

1. Mutability
2. Heap and memory
3. Ownership model
4. Borrowing and references
5. lifetime

### Mutability

In Rust the variables are always immutable. once the variable is created the we cannot add or update the values as they are immutable. We can explicitly make the variable mutable by adding the `mut` keyword before the variable

```rust
fn main() { 
	let mut x: i32 = 1; 
	x = 2; // No error println!("{}", x); 
}
```

In the rust all the variables are immutable because

1. Immutable data is inherently thread-safe because if no thread can alter the data, then no synchronization is needed when data is accessed concurrently.
2. Knowing that certain data will not change allows the compiler to optimize code better.

### Stack vs. Heap Allocation

Rust  has clear rules about stack and heap data management.

- stack: Fast allocation and deallocation. Rust uses the stack for most primitive data types and for data where the size is known at compile time
- Heap: Used for data that can grow at runtime, such as vectors or strings.

#### Stack:

- The **stack** is used for data with a **known, fixed size at compile time**.
- This includes **literals** like integers (`i32`, `u64`), floats (`f64`), booleans (`bool`), and fixed-size arrays or structs.
- Values stored on the stack are stored directly, and their size is known upfront.
- Stack allocation is very fast and follows a Last-In-First-Out (LIFO) order.

#### Heap:

- The **heap** is used for **dynamically sized data** or data where the size is not known at compile time.
- Examples are:
  - `String` (the actual string data is stored on the heap; the `String` object on the stack contains a pointer, length, and capacity)
  - `Vec<T>` (a growable array; its elements are stored on the heap)
  - `Box<T>` (a smart pointer that allocates data on the heap)
- Heap allocation is slower than stack allocation because it requires managing memory (allocation and deallocation).

Example:
When we create an variable with numbers or literals

![[static_sized_values.png]]

#### String (Heap Example)

![[dynamic_sized_values.png.png]]
### Own