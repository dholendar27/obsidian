---
date created: 2025-10-27 12:14
---

## 1. `number`

TypeScript has only one number type, which represents both **integers** and **floating-point numbers** (like JavaScript).
It also supports **binary**, **octal**, and **hexadecimal** literals.

### Example

```ts
let decimal: number = 42;
let hex: number = 0xf00d;     // hexadecimal
let binary: number = 0b1010;  // binary
let octal: number = 0o744;    // octal
let pi: number = 3.14159;
```

### Use Case

Whenever you work with arithmetic, percentages, or measurements.

---

## 2. `string`

Used to represent textual data.
You can use **single (`'`)**, **double (`"`)**, or **template literals (\`\`\`)**.

### Example

```ts
let firstName: string = "Alice";
let lastName: string = 'Smith';
let greeting: string = `Hello, ${firstName} ${lastName}!`;
```

### Use Case

For representing and manipulating text, names, or any string-based data.

---

## 3. `boolean`

Represents a logical value: either `true` or `false`.

### Example

```ts
let isActive: boolean = true;
let isLoggedIn: boolean = false;

if (isActive) {
  console.log("User is active!");
}
```

### Use Case

Used in conditional logic, state toggles, flags, etc.

---

## 4. `null` and `undefined`

### Description

- `null` represents _intentional absence_ of a value.
- `undefined` means a variable has been declared but not assigned a value.

### Example

```ts
let n: null = null;
let u: undefined = undefined;
let notAssigned: number | undefined; // can be undefined until assigned
```

### Note

With the compiler option `--strictNullChecks`, these are treated as **separate types**, not automatically assignable to others.

---

## 5. `any`

Disables all type checking for that variable — it can hold **any type of value**.
This is useful for gradually migrating JavaScript code to TypeScript, but it **removes type safety**.

### Example

```ts
let data: any = 42;
data = "Hello"; // no type error
data = { key: "value" };
data = false;
```

### Warning

Use `any` **only when necessary**, as it defeats TypeScript’s purpose of catching type errors.

---

## 6. `unknown`

`unknown` is similar to `any`, but **safer** — you must check its type before using it.

### Example

```ts
let value: unknown = "Hello TypeScript";

if (typeof value === "string") {
  console.log(value.toUpperCase()); // ✅ Safe after check
}
```

### Use Case

When dealing with external data (like API responses) where you don’t know the type upfront.

---

## 7. `void`

### Description

Represents the **absence of a return value**, typically used for functions that don’t return anything.

### Example

```ts
function logMessage(message: string): void {
  console.log(message);
}

let result: void = logMessage("Hi!"); // result is void
```

### Use Case

Used for event handlers, logging, or other side-effect functions.

---

## 8. `never`

### Description

Indicates that a function **never returns**.\
This could be because it always **throws an error** or **loops forever**.

### Example

```ts
function throwError(message: string): never {
  throw new Error(message);
}

function infiniteLoop(): never {
  while (true) {
    console.log("Running...");
  }
}
```

### Use Case

Used for exhaustive type checking or functions that can’t logically reach an endpoint.

---

## 9. `symbol`

### Description

Introduced in ES6 — symbols are unique and immutable identifiers, often used as object keys to avoid name collisions.

### Example

```ts
let sym1 = Symbol("id");
let sym2 = Symbol("id");

console.log(sym1 === sym2); // false (symbols are always unique)
```

### Use Case

Used in advanced cases — e.g., hiding internal object properties or implementing iterators.

---

## 10. `bigint`

### Description

Represents **very large integers** beyond the safe range of JavaScript’s `number` type (`2^53 - 1`).

### Example

```ts
let largeNumber: bigint = 9007199254740991n;
let anotherBig: bigint = BigInt(123456789012345678901234567890n);

console.log(largeNumber + 1n);
```

### Use Case

Used in cryptography, financial calculations, or precise arithmetic on large numbers.
