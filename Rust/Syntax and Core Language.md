---
date created: 2025-10-19 15:23
date updated: 2025-10-19 16:05
---

## Variables and immutability

In the rust the variables are declared using the `let` keyword.

```rust
let x = 5;
```

By default the variables in the rust are immutable, once they are assigned they cannot be changed.

```rust
let x = 5;
x = 6; // Error: cannot assign twice to immutable variable 'x'
```

This prevents accidental changes to variables and help avoid bugs related to shared mutable state.

#### Making the variable Mutable

To allow a variable to be changed, we need to explicitly make it mutable using the `mut` keyword:

```rust
let mut x = 5;
x = 6
```

#### Shadowing: Re-binding a Variable

Rust allow shadowing - redeclaring a variable with same name, which creates a new variable, even if the original is immutable

```rust
let x = 5;
let x = x + 1; // New `x` shadows the old one
```

#### constants

Rust also supports constants using the `const` keyword

```rust
const MAX_POINTS: u32 = 100_000;
```

##### Difference between `let` and `const`

| Feature    | `let`                              | `const`                         |
| ---------- | ---------------------------------- | ------------------------------- |
| Mutability | Immutable by default; can be `mut` | Always immutable                |
| Scope      | Function-local or block-local      | Global or scoped                |
| Value      | Can be any expression              | Must be a compile-time constant |
| Type       | Inferred or specified              | Must be explicitly typed        |

## DataTypes

Rust is an statically types language, which means that it must know the types of all variable at compile time.

> Rust compiler need to know the type of the variable at compile time

The Datatypes in rust are categorized into:

### Scalar types

Scalar types represent a single value, Rust has four primary scalar types:

#### Integer Types:

Used to represent whole number

| Bits    | Signed Type | Unsigned Type |
| ------- | ----------- | ------------- |
| 8-bit   | `i8`        | `u8`          |
| 16-bit  | `i16`       | `u16`         |
| 32-bit  | `i32`       | `u32`         |
| 64-bit  | `i64`       | `u64`         |
| 128-bit | `i128`      | `u128`        |
| Arch    | `isize`     | `usize`       |

- **Signed** integers (`i32`, etc.) can store both negative and positive values.
- **Unsigned** integers (`u32`, etc.) can store only positive values.

#### Floating-Point Types

For numbers with decimals.

- `f32`: 32-bit float
- `f64`: 64-bit float (default)

#### Boolean

- Type: `bool`
- Values: `true` or `false`

#### Character

- Type: `char`
- Represents a Unicode scalar value.
- Example: `'a'`, `'ℤ'`, `'😊'`

### Compound Types

Compound types group multiple values into one type

#### Tuple

A collection of values of different types.

```rust
let tup: (i32, f64, char) = (500, 6.4, 'z');
let (x, y, z) = tup;
let first = tup.0; // 500
```

#### Array
A fixed-size collection of elements of the same type

```rust 
let arr: [i32; 3] = [1, 2, 3];
let first = arr[0]; // 1
```

- Arrays are stack-allocated.
- Use slices (`&[i32]`) for referencing parts of an array.