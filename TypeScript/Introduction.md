---
date created: 2025-10-27 10:08
date updated: 2025-10-27 10:46
---

TypeScript is a **strongly typed**, **object-oriented** language developed by **Microsoft**. It is essentially a **superset of JavaScript**, which means that TypeScript includes all features of JavaScript with additional features, notably **static types** and **type-checking**.

Because it is a superset of JavaScript, **every valid JavaScript** code is also valid TypeScript. However, TypeScript introduces optional types, interfaces, classes, and other features to improve code reliability, readability, and maintainability.

#### Key Benefits of TypeScript

- **Type Safety**: Helps catch errors at compile time rather than runtime.
- **Object-Oriented Programming**: Supports modern OOP features such as classes and interfaces.
- **Tooling Support**: TypeScript offers better auto-completion, navigation, and refactoring tools compared to plain JavaScript.

### Installing TypeScript

To install TypeScript globally on your system, you can use the following command:

```bash
npm install -g typescript
```

This will allow you to use the `tsc` command to compile TypeScript files anywhere on your system.

### Compiling TypeScript Code

To compile TypeScript to JavaScript, run:

```bash
tsc main.ts
```

This will generate a `main.js` file from the `main.ts` file. If you don't want to recompile manually every time you change the TypeScript code, you can add a **watch mode**:

```bash
tsc main.ts -w
```

The `-w` flag starts a watch process that monitors your TypeScript file for changes and automatically recompiles the file whenever it is updated.

---

### Example Breakdown

#### TypeScript Code:

```typescript
let name: string = "Dave";  // Type annotation (string)
console.log("My name is", name);
```

#### Compiled JavaScript (main.js):

```javascript
var name = "Dave";  // JavaScript version without type annotations
console.log("My name is", name);
```

As you can see, TypeScript compiles down to plain JavaScript, which can then run in any JavaScript environment (browser, Node.js, etc.).

## What is `tsconfig.json`?

The **`tsconfig.json`** file is the TypeScript configuration file that controls how the TypeScript compiler (`tsc`) processes your code. This file can define compiler options, include/exclude certain files, specify the output location for compiled JavaScript, and other settings.

```json
{
  "compilerOptions": {
    "target": "es6",                 // Specify ECMAScript target version (e.g., es5, es6)
    "module": "commonjs",            // Specify module system (e.g., commonjs, esnext, amd)
    "outDir": "./dist",              // Directory where compiled JS will go
    "rootDir": "./src",              // Root directory of TypeScript files
    "strict": true,                  // Enable all strict type-checking options
    "esModuleInterop": true,         // Allows default import of CommonJS modules
    "skipLibCheck": true,            // Skip type-checking of declaration files
    "forceConsistentCasingInFileNames": true // Ensure consistent file naming case sensitivity
  },
  "include": [
    "src/**/*"                       // Include all files in the `src/` folder
  ],
  "exclude": [
    "node_modules",                  // Exclude the `node_modules/` folder
    "dist"                           // Exclude the `dist/` folder
  ]
}
```

### Key Fields in `tsconfig.json`:

1. **`compilerOptions`**:
   - This is the heart of the configuration, where you define how the TypeScript compiler should treat the source files.
   - **`target`**: Specifies the target ECMAScript version to which TypeScript should transpile. Common options include `es5`, `es6`, or `esnext`.
   - **`module`**: Defines the module system to use in the output JavaScript code. For example, `commonjs` is used for Node.js projects, and `esnext` is used when working with modern JavaScript modules.
   - **`outDir`**: Specifies the directory where the compiled JavaScript files should go (usually `dist` or `build`).
   - **`rootDir`**: Defines the root directory for your TypeScript files (usually `src`).
   - **`strict`**: Enabling this flag turns on a set of strict type-checking options (e.g., `noImplicitAny`, `strictNullChecks`, etc.), making your code safer by catching more errors during compilation.
   - **`esModuleInterop`**: Ensures compatibility when importing CommonJS modules using `import` statements.
2. **`include`**:
   - This is an array of file patterns (usually glob patterns) to specify which files to include in the TypeScript project. Typically, you'll include all files in the `src/` folder.
3. **`exclude`**:
   - This is an array of file patterns to specify files or folders that should not be compiled by TypeScript. Common entries are `node_modules/` and `dist/`.

To generate `tsconfig.json`. run:

```bash
tsc --init
```

