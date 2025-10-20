---
date created: 2025-10-19 15:23
date updated: 2025-10-20 10:17
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

## Operators and expressions

Rust supports several types of operators:

#### 1. **Arithmetic Operators**

- `+` Addition
- `-` Subtraction
- `*` Multiplication
- `/` Division
- `%` Remainder (modulus)

```rust
let a = 10;
let b = 3;
let sum = a + b;        // 13
let diff = a - b;       // 7
let prod = a * b;       // 30
let div = a / b;        // 3 (integer division)
let rem = a % b;        // 1
```

---

#### 2. **Comparison Operators**

- `==` Equal to
- `!=` Not equal to
- `<` Less than
- `>` Greater than
- `<=` Less than or equal to
- `>=` Greater than or equal to

```rust
let is_equal = a == b;  // false
let less = a < b;       // false
```

---

#### 3. **Logical Operators**

- `&&` Logical AND
- `||` Logical OR
- `!` Logical NOT

```rust
let x = true;
let y = false;
let result = x && y;    // false
let result2 = x || y;   // true
let not_x = !x;         // false
```

---

#### 4. **Bitwise Operators**

- `&` Bitwise AND
- `|` Bitwise OR
- `^` Bitwise XOR
- `<<` Left shift
- `>>` Right shift

```rust
let x = 0b1010;         // 10 in decimal
let y = 0b1100;         // 12 in decimal
let and = x & y;        // 0b1000 (8)
let or = x | y;         // 0b1110 (14)
let xor = x ^ y;        // 0b0110 (6)
let left_shift = x << 2;  // 0b101000 (40)
```

---

#### 5. **Assignment Operators**

- `=` Simple assignment

- `+=`, `-=`, `*=`, `/=`, `%=` Compound assignment operators

```rust
let mut a = 5;
a += 3;  // a = 8
a *= 2;  // a = 16
```

---

### Expressions in Rust

- **Expressions** are chunks of code that evaluate to a value.
- Most constructs in Rust are expressions.
- Expressions do **not** include statements like `let` bindings or `return`.

Example of expressions:

```rust
let x = 5 + 6;  // 5 + 6 is an expression resulting in 11

let y = {
    let a = 3;
    a + 1        // The last expression without a semicolon is the block's value
};
```

Note: Adding a semicolon turns an expression into a statement and discards the value.

---

## Strings

A **string** is just some **text**, like `"hello"`, `"world"`, or `"Rust is awesome"`.
Rust has two main types of strings:

### String -> `owned`,`Growable Text`

- The string literal `"hello"` is stored in the program’s **read-only memory** inside the binary (this is part of the program’s static data).
- When you call `String::from("hello")`, Rust **copies** this `"hello"` data from the read-only memory into a new **block of memory on the heap**.
- The variable `s` itself lives on the **stack**, but it contains:
  - A **pointer** to the heap memory where `"hello"` is stored,
  - The **length** of the string (5),
  - And the **capacity** (how much memory is allocated, can be bigger than length).

```rust
let mut s = String::from("Hello");
s.push_str(" world!");
println!("{}", s); // Output: Hello world!
```

### &str -> `String Slice (Read-only)`
```rust
fn main() {
    let greeting = "Hello, world!"; // &str
    println!("{}", greeting);
}
```

`"Hello, world!"` is a **string literal** stored in the program’s **read-only memory**.
- The `&str` itself **does not own** the data.
- And the data behind `&str` is **fixed size** and **not growable**.
- You can only read from it, not change or grow it.