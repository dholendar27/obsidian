---
date created: 2025-10-20 12:42
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
