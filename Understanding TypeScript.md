# Understanding TypeScript

## Section 1 Getting Started

### What is TypeScript

- A JavaScript super set
- A language building upon JavaScript
- TypeScript cannot be execute by JavaScript environment like the browser
- TypeScript need to be compiled to JavaScript
- Identify error earlier before it occurs in the run-time

### TypeScript Advantages

- Types
- Next-gen JavaScript Features
- Non-JavaScript features like Interfaces and Generics
- Meta-Programming Features like Decorators
- Rich configuration options

## Section 2 TypeScript Basics and Basic Types

### Core Types

- number
- string
- boolean
- object
- Array
- Tuple ```role: [number, string]``` (fixed length and fixed type [2, "author"])
- Enum
- any

### Type Assignment

```typescript
// Bad practice because const never changes
const a: number = 5;
// Good practice
let b: number;
b = 5
```

### Union Types

```typescript
let input: number | string;
```

### Literal Types

```typescript
let input : "Author" | "Admin";
```

### Type Aliases/Custom Types

```typescript
type Combinable = number | string;
function combine (input1: Combinable, input2: Combinable){}
```

### Functions as Types

```typescript
let myFunctionA: Function;
// myFunctionB will take two params which are number and return a number
let myFunctionB: (a: number, b: number) => number
```

### Function Types and Callbacks

```typescript
// cb return void will actually ignore what may return in the cb function
function addAndHandle (a: number, b: number, cb: (num: number) => void){
    const result = a + b;
    cb(result);
}

addAndHandle(10, 20, (result)=>{
    // This is legal because we can return in the cb function
    return result.toString();
})
```

### The "unknown" Type

```typescript
let userInput = unknown;
let userName: string;

userInput = 5;
unserInput = "Hello";

// Error because unserInput type is unknown
userName = userInput;

// This will work because we check the userInput type and make sure it is string
if(typeof userInput === "string"){
    userName = userInput;
}
// From the typing perspective, unknown is better than using any.
```

### The "never" Type

```typescript
function generateError(message: string, code: number): never {
    throw {message: message, code: code};
}
// If we try:
const result = generateError("An error occured!", 500);
console.log(result); // This will log nothing, not even undefined.
// So, we use never to type our generateError function.
```

## Section 3 The TypeScript Compiler

### Using "Watch Mode"

```bash
tsc app.ts -w
```

### Compiling the Entire Project

```bash
tsc --init
tsc -w
```

```json
"exclude": [
	"some.ts",
    "*.dev.ts",
    "node_modules" // would be the default
],
"include":[
    "some1.ts",
    "mydir"
],
"files":[
    "some2.ts"
],
"noEmitOnError": false, // if there are errors in TS file, no JS file will be compiled
```

## Section 4 Next-generation JavaScript & TypeScript

## Section 5 Classes & Interfaces

### Key Concepts

- Constructor
- Access Modifiers (public private protected)
- Shorthand Initialization
- readonly
- Inheritance (super)
- Overriding
- Getter & Setter
- Static properties and methods
- Abstract class
- Singletons & private constructors

### Shorthand Initialization

```typescript
class Department {
    private employees: string[] = [];
    constructor(private readonly id: number, public name: string){
        
    }
}

const accountingDepartment = new Department(1, "Accounting");
```

### Getter & Setter

```typescript
class User {
    
    constructor(private name: string){
        
    }
    
    get getUsername(){
        return this.name;
    }
}

const usera = new User("Hello");
// not usera.getUsername()
console.log(usera.getUsername)
```

### Abstract Class

```typescript
abstract class MyClass{
    //...
    
    abstract myFun(): void;
}
```

### Singletons & Private Constructors

You only want one single instance/object of a class.

```typescript
// I only want one Accounting Department
// Since we cannot initiate a new instance, we use static methods
class AccountingDepartment {
    private constructor(){
        
    }
    static myFun(){
        
    }
}
```

### Using Interfaces

- Interfaces are more often to describe object.
- Implement by classes
- Interfaces have no implementations at all comparing with abstract classes

```typescript
interface Greetable {
    name: string;
    greet(phrase: string): void;
}

class Person implements Greetable {
    name: string;
    age: number
    
    constructor(n: string, a: number){
        this.name = n;
        this.age = number;
    }
    
    greet(phrase: string){
        console.log(phrase + " " + this.name);
    }
}
```

### Interfaces vs Types

- Similarity

  ```typescript
  // Can type a object or function
  interface User{
      name: string;
      age: number;
  }
  
  interface SetUser {
      (name: string, age: number): void;
  }
  
  type User = {
      name: string;
      age: number;
  }
  
  type SetUser = (name: string, age: number) => void;
  ```

  ```typescript
  // Can extends
  
  // interface extends interface
  
  interface Name {
      name: string;
  }
  interface User extends Name {
      age: number;
  }
  
  // type extends type
  
  type Name = {
      name: string;
  }
  
  type User = Name & {age: number}
  
  // interface extends type
  type Name = {
      name: string;
  }
  
  interface User extends Name {
      age: number;
  }
  
  // type extends interface
  
  interface Name {
      name: string;
  }
  
  type User = Name & {age: number};
  ```

  - Differences

  ``````typescript
  // only for type
  type Name = string; 
  
  interface Dog {
      wong();
  }
  
  interface Cat {
      miao();
  }
  
  type Pet = Dog | Cat;
  type PetList = [Dog, Cat];
  
  // typeof
  
  const div = document.createElement('div');
  type Div = typeof div;
  ``````

  ```typescript
  // only for interface
  
  interface User {
      name: string;
      age: number
  }
  
  interface User {
      gender: string;
  }
  ```

### Readonly Interface

```typescript
interface User {
    readonly id: number
}
```

## Section 6 Advanced Types

### Intersection Types

```typescript
type TypeA = {
    prop1: string;
    prop2: number;
};

type TypeB = {
    prop1: string;
    prop3: boolean;
}

type TypeAB = TypeA & TypeB;

// Similarly, we can use interface

interface InterfaceA {
    prop1: string;
    prop2: number;
}

interface InterfaceB {
    prop1: string;
    prop3: boolean;
}

interface InterfaceAB extends InterfaceA, InterfaceB;
```

