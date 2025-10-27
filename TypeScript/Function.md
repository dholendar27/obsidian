---
date created: 2025-10-27 15:51
---

### 1. **Function Declaration**

A function in TypeScript can be declared just like in JavaScript but with added type annotations for parameters and the return type. Here's a basic example:

```typescript
function add(a: number, b: number): number {
    return a + b;
}
```

- **Parameters**: `a: number, b: number` — the type of `a` and `b` is specified as `number`.
- **Return Type**: `: number` — the return type of the function is specified as `number`.

In this case, TypeScript ensures that `add` can only accept numbers as arguments and that it returns a number.

### 2. **Function Expressions**

Functions can also be defined using **function expressions**. This is often used with anonymous functions or lambda-style functions (arrow functions). In TypeScript, we can specify types in function expressions as well.

```typescript
const multiply = (a: number, b: number): number => {
    return a * b;
};
```

### 3. **Optional Parameters**

You can specify optional parameters by appending a `?` to the parameter name. These parameters must come after all required parameters.

```typescript
function greet(name: string, age?: number): string {
    if (age) {
        return `Hello, ${name}! You are ${age} years old.`;
    } else {
        return `Hello, ${name}!`;
    }
}
```

In the above example, `age` is optional. If it's not passed, it will be `undefined`.

### 4. **Default Parameters**

You can also specify default values for parameters. If the caller doesn’t provide a value, the default will be used.

```typescript
function greet(name: string, age: number = 25): string {
    return `Hello, ${name}! You are ${age} years old.`;
}
```

In this case, if no `age` is passed, it defaults to `25`.

### 5. **Rest Parameters**

If you need to accept a variable number of arguments, you can use **rest parameters**. The rest parameter is preceded by `...` and must be the last parameter in the function.

```typescript
function sum(...numbers: number[]): number {
    return numbers.reduce((total, num) => total + num, 0);
}
```

Here, `numbers` is an array of type `number[]`, and you can pass as many numbers as you like to the function.

### 6. **Function Overloading**

TypeScript supports **function overloading**, allowing you to define multiple function signatures with different parameter types or return types. However, only one implementation is allowed.

```typescript
function greet(person: string): string;
function greet(person: string, age: number): string;
function greet(person: string, age?: number): string {
    if (age) {
        return `Hello, ${person}! You are ${age} years old.`;
    } else {
        return `Hello, ${person}!`;
    }
}
```

- The first two signatures are overloads. They define the allowed parameter types and return types for different scenarios.
- The implementation, which follows the overloads, combines both use cases.

### 7. **Type Aliases for Functions**

You can define a **type alias** for a function type. This is especially useful when passing functions as parameters or when defining complex callback functions.

```typescript
type Operation = (a: number, b: number) => number;

const add: Operation = (a, b) => a + b;
const subtract: Operation = (a, b) => a - b;
```

Here, the `Operation` type defines a function signature that takes two `number` arguments and returns a `number`. Both `add` and `subtract` adhere to this type.

### 8. **Arrow Functions**

Arrow functions (`=>`) offer a more concise way to write functions. They also don’t have their own `this` context; instead, they inherit the `this` from the enclosing scope.

```typescript
const greet = (name: string) => `Hello, ${name}!`;
```

This is equivalent to the longer function declaration:

```typescript
function greet(name: string): string {
    return `Hello, ${name}!`;
}
```
