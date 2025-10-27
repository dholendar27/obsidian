---
date created: 2025-10-26 06:48
date updated: 2025-10-26 07:23
---

## Variables

A `Variable` is a "named storage" for data. we can use variables to store goodies, visitors and other data.

## Variable Declaration

In javascript variables are declared using three keywords. They are

### let

The let was introduced in the es6, It is an block scoped. it can be updated but cannot be declared within the same scope.

```javascript
let city = "newyork";
city = "Bangalore";
let city = "California"; // Not allow in same scope
```

### Var

- **Function-scoped** — visible only inside the function where declared.
- Can be **redeclared** and **reassigned**.
- **Not block-scoped**, meaning it leaks out of `{}` blocks.

```javascript
var city = "London";
console.log(city); // London
city = "Paris"; // reassign
console.log(city); // Paris

var city = "Tokyo"; // redeclare allowed
console.log(city); // Tokyo
```

### Const

- **Block-scoped**
- **Must** be initialized when declared
- Cannot be **redeclared** or **reassigned**
- Ideal for constants or values that never change

```javascript
const PI = 3.14159;
console.log(PI);
```

### Scope

Scope means where a variable can be access in our code.
It determine the visibility and lifetime of a variable.
In Javascript there are three types of scopes:

1. Global Scope
2. Function Scope
3. Block Scope

#### 1. Global Scope

If a variable is declared outside any function or block, it's global -it can be accessed anywhere in our code. But overusing the global variable cause naming conflicts.

#### 2. Function Scope

If a variable is declared inside function, it only visible inside that function - it cannot be used outside.

#### 3. Block Scope

`let` and `const` are block-scoped, meaning they exists only inside `{}` braces

```javascript
let globalVariable = "I'm a global variable";
{
  let BlockLevelVariable = "I'm a block level variable";
  console.log(globalVariable);
  console.log(BlockLevelVariable);
}
console.log(BlockLevelVariable); // ReferenceError: BlockLevelVariable is not defined
```

## Hoisting

Hoisting is javascripts behaviour of moving variable and function declarations to the top of their scope during compilation.

#### Example with `var`:

```javascript
console.log(a); // undefined (not an error!)
var a = 5;
```

Internally, JavaScript does this:

```javascript
var a;
console.log(a); // undefined
a = 5;
```

#### Example with `let` and `const`:

```javascript
console.log(b); // ReferenceError
let b = 10;
```

This happens because `let` and `const` are **hoisted**, but **not initialized** — they are in the **Temporal Dead Zone (TDZ)** until their declaration line is reached.