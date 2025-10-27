## **1. Arrays in TypeScript**

Arrays are used to store multiple values of the same type. In TypeScript, you can define the type of elements in an array for type safety.

**Syntax:**

```ts
let numbers: number[] = [1, 2, 3, 4];
let names: string[] = ["Alice", "Bob"];
```

**Alternative syntax using generics:**

```ts
let numbers: Array<number> = [1, 2, 3];
let names: Array<string> = ["Alice", "Bob"];
```

**Mixed types using union types:**

```ts
let mixed: (string | number)[] = [1, "hello", 2, "world"];
```

**Common array operations (with type safety):**

```ts
numbers.push(5); // allowed
// numbers.push("hello"); // Error: Type 'string' is not assignable to type 'number'
```

---

## **2. Tuples in TypeScript**

Tuples are fixed-length arrays where each element can have a different type. They are useful when you know exactly how many elements you need and their types.

**Syntax:**

```ts
let person: [string, number] = ["Alice", 25];
```

**Accessing tuple elements:**

```ts
let name = person[0]; // "Alice"
let age = person[1]; // 25
```

**Tuple with optional and rest elements:**

```ts
let employee: [string, number, ...string[]] = ["John", 30, "Manager", "HR"];
```

---

## **3. Objects in TypeScript**

Objects store key-value pairs. TypeScript allows you to define the shape (types of properties) of an object.

**Basic object type:**

```ts
let person: { name: string; age: number } = {
    name: "Alice",
    age: 25
};
```

**Accessing object properties:**

```ts
console.log(person.name); // Alice
console.log(person.age);  // 25
```

**Optional properties:**

```ts
let person: { name: string; age?: number } = { name: "Bob" };
// age is optional
```

**Index signatures (dynamic keys):**

```ts
let scores: { [key: string]: number } = { math: 90, english: 85 };
scores["science"] = 95;
```

---

## **4. Type Aliases in TypeScript**

Type aliases allow you to create custom types for primitives, objects, unions, tuples, etc. They are reusable and improve code readability.

**Basic example:**

```ts
type User = {
    name: string;
    age: number;
};

let user: User = { name: "Alice", age: 25 };
```

**Union types using type alias:**

```ts
type ID = string | number;
let userId: ID = 123;
userId = "ABC"; // allowed
```

**Tuple with type alias:**

```ts
type Employee = [string, number];
let emp: Employee = ["John", 30];
```

---

## **5. Interfaces in TypeScript**

Interfaces are similar to type aliases for defining object shapes, but they have extra capabilities like **extending other interfaces**.

**Basic interface:**

```ts
interface User {
    name: string;
    age: number;
}

let user: User = { name: "Alice", age: 25 };
```

**Optional properties:**

```ts
interface User {
    name: string;
    age?: number;
}
```

**Read-only properties:**

```ts
interface User {
    readonly id: number;
    name: string;
}

let user: User = { id: 1, name: "Alice" };
// user.id = 2; // Error: Cannot assign to 'id' because it is a read-only property
```

**Extending interfaces:**

```ts
interface Person {
    name: string;
}

interface Employee extends Person {
    salary: number;
}

let emp: Employee = { name: "John", salary: 5000 };
```

**Difference between type and interface:**

|Feature|Type Alias|Interface|
|---|---|---|
|Objects|Yes|Yes|
|Primitives|Yes|No|
|Tuples|Yes|No|
|Extendable|Can use intersections (`&`)|Can extend other interfaces|
|Declaration merging|No|Yes|

**Example of type intersection:**

```ts
type Person = { name: string };
type Contact = { email: string };

type Employee = Person & Contact;
let emp: Employee = { name: "Alice", email: "alice@mail.com" };
```

## Enums

Enums are a **TypeScript feature** that allows you to define a set of named constants. They make your code more readable and maintainable compared to using plain numbers or strings.

#### Syntax Example:

```ts
enum Direction {
  Up,
  Down,
  Left,
  Right
}
```

Here:

- `Direction.Up` is `0`
- `Direction.Down` is `1`
- `Direction.Left` is `2`
- `Direction.Right` is `3`

TypeScript **automatically assigns numeric values starting from 0** unless you specify them.

---

### Numeric Enums with Custom Values:

```ts
enum StatusCode {
  Success = 200,
  NotFound = 404,
  ServerError = 500
}
```

---

### String Enums:

```ts
enum Color {
  Red = "RED",
  Green = "GREEN",
  Blue = "BLUE"
}
```

---

### Key Features of Enums:

1. **Improved readability** – Instead of remembering numbers or strings, you use descriptive names.
2. **Auto-incremented values** – Numeric enums auto-increment if you don’t set the value explicitly.
3. **Reverse mapping** – Numeric enums allow you to get the name from the value:
    

```ts
enum Direction {
  Up = 1,
  Down,
  Left,
  Right
}

console.log(Direction[2]); // Output: Down
```
