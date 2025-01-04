# Typescript By Hitesh(Youtube)

**Reference Link** : [Youtube Link](https://www.youtube.com/playlist?list=PLRAV69dS1uWRPSfKzwZsIm-Axxq-LxqhW)

## Provides Type Safety

Example:

```javascript
2 + "2" // Outputs: '22' in JavaScript
```

Typescript helps prevent such issues with type safety.

## What does TypeScript do?

- **Static Checking**: Catches errors at compile-time instead of runtime.
- TypeScript is a development tool; the project still runs in JavaScript.

## Installation

Refer to the official TypeScript documentation for installation instructions. [docs](https://www.typescriptlang.org/download/)

## Running a TypeScript File

```bash
tsc index.ts
```

This command compiles the TypeScript file into a JavaScript file (`index.js`).

## Basic Syntax

```typescript
let variableName: type = value;
```

### Type Inference

In many cases, TypeScript can infer the type. For example:

```typescript
let userId = 3; // TypeScript infers 'userId' as a number.
```

## Functions

### Basic Function

```typescript
function addTwo(num: number): number {     return num + 2; }
```

Or using an arrow function:

```typescript
const addTwo = (num: number): number => num + 2;
```

### Objects in Functions

```typescript
function createUser({name: string, isPaid: boolean}): {name: string, price: boolean} {     
    return {name: "harsh", price: false}; 
}
```

## Working with Types

You can define types for complex data structures.

```typescript
type User = {     
        name: string;     
        email: string;     
        isActive: boolean; 
        };  
function createUser(user: User) {     
    // Function logic here 
    }
```
