# Understanding TypeScript

## Section 1 Getting Started

### What is TypeScript

- A JavaScript super set
- A language building upon JavaScript
- TypeScript cannot be executed by JavaScript environment like the browser
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
    age: number;
    
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
      age: number;
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

### Type Guards

```typescript
class Foo {
    
}
const f1 = new Foo();
if(f1 instanceof Foo){
    //do something
}
// instanceof can only work on class not interface
```

### Discriminated Unions

```typescript
interface Dog {
    type: "dog";
    runningSpeed: number;
}

interface Bird {
    type: "bird"
    flyingSpeed: number;
}

// Now we can check if an instance has type:"dog" or type:"bird" to tell which interface it belongs to.
```

### Type Casting

```typescript
// 1
const userInputElement = <HTMLInputElement>document.getElementById("user-input");
const userInput = userInputElement.value;

//2
const userInputElement = document.getElementById("user-input") as HTMLInputElement;
const userInput = userInputElement.value;
```

### Index Properties

```typescript
interface ErrorContainer {
    [prop: string]: string
}

const errorBag: ErrorContainer = {
    email: "Not a valid email!"
}
```

### Function Overload

```typescript
type Combinable = string | number;

function add(a: Combinable, b: Combinable ){
    if(typeof a === "string" && typeof b === "string"){
        return a.toString() + b.toString();
    }
    return a + b;
}

const result = add("Weikun", "Ye");
// Cannot access "split" even though we know result will be a string
result.split(' ');

// So we use function overload
function add(a: number, b: number ): number;
function add(a: string, b: string ): string;
function add(a: Combinable, b: Combinable ){
	//...
}
// We tell TS if a is number and b is number, function will return number
// and if a is string and b is string, function will return string
```

### Optional Chaining

```typescript
const user = {
    name: "xxx",
    job: {
        title: "CEO",
        Company: "xxx"
    }
}
// job property may be optional
// when we fetch from backend, we don't know if an user have a job
console.log(user.job?.title)
```

### Nullish Coalescing

```typescript
// we get userInput from somewhere (frontend)
// we don't know what data we get
const userInput = "xxx";
const userInput = '';
const userInput = null;
const userInput = undefined;

// we want to check if userInput is null or undefined, we store DEFAULT
// else we store the incoming data
const storeData = userInput ?? "DEFAULT";

// note: if we use following checking, a empty string '' will be treated as false too.
// so we cannot store the empty string but "DEFAULT"
const const storeData = userInput || "DEFAULT";
```

## Generics

### Generic Functions

```typescript
function merge(a: object, b: object){
    return Object.assign(a, b)
}
const mergedObj = merge({name: "sss"}, {age: 15});
// if we access the properties of mergedObj
// it will return error
// this will have an error because TS doesn't know name and age are in mergedObj
console.log(mergedObj.name) 

// Then we need
function merge<T, U>(a: T, b: U){
    return Object.assign(a, b)
}
// Then we can access mergedObj.name and mergedObj.age 
```

### Constraints

```typescript
function merge<T extends object, U extends object>(a: T, b: U){
    return Object.assign(a, b)
}
// also can extends to custom interface
```

### "typeof" Constraints

```typescript
// this will give us an error because TS don't know whether there is a key in the obj
function getValueFromKey (obj: object, key: string){
    return obj[key];
}
// use generics constraints
function getValueFromKey<T extends obj, U extends keyof T> (obj: T, key: U){
    return obj[key];
}
```
### Generics Class
```typescript
// this will only work for primative types
class DataStorage<T>{
    foo(item: T){
        // do something here
    }
}

const stringStorage = new DataStorage<string>();
```
### Generic Utility Types
```typescript
// Partial
interface User {
    name: string;
    age: number;
    job: string;
}

function createUser(name: string, age: number, job: string): User {
    let user: Partial<User> = {};
    user.name = name;
    user.age = age;
    user.job = job;

    return user as User;
}
// Readonly
const name: Readonly<string[]> = {"Jack", "Wayne"};
// name.push("Lily") will not work
```
## Section 8 Decorators
### Class Decorators
```typescript
function Logger(constructor: Function){
    console.log("Logging...");
    console.log(constructor);
}

@Logger
class Person {
    name = "Wayne";

    constructor(){
        console.log("Creating person...");
    }
}
```
### Decorator Factories
```typescript
function Logger(logString: string){
    return function(constructor: Function){
        console.log(logString);
        console.log(constructor);
    }
}

@Logger('LOGGING - PERSON')
class Person {
    name = "Wayne";

    constructor(){
        console.log("Creating person...");
    }
}
```
### Multiple Decorators
- Bottom up
- Decorator factories run first

## Modules & Namespaces
## Using Webpack with Typescript
### What is Webpack and Why we need it?
- It is a bundling and "build orchestration" tool
- Code bundles, less imports required
- Optimized (minified) code, less code to download
- More build steps can be added easily
### Install and config Webpack
```bash
yarn add webpack webpack-cli webpack-dev-server typescript ts-loader -D
```
```javascript
//webpack.config.js
const path = require('path');
module.exports = {
    mode: 'development',
    entry: './src/app.ts',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist'),
        publicPath: 'dist',
    },
    devtool: 'inline-source-map',
    module: {
        rules: [
            {
                test: /\.ts&/,
                use: 'ts-loader',
                exclude: /node_modules/
            }
        ]
    },
    resolve: {
        extensions: ['.ts', '.js']
    }
}
```
```json
{
    "build": "webpack",
    "start": "webpack-dev-server",

}
```
### Adding production workflow
```bash
yarn add -D clean-webpack-plugin
```
```javascript
//webpack.config.prod.js
const path = require('path');
const CleanPlugin = require('clean-webpack-plugin');
module.exports = {
    mode: 'production',
    entry: './src/app.ts',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist')
    },
    devtool: 'none',
    module: {
        rules: [
            {
                test: /\.ts&/,
                use: 'ts-loader',
                exclude: /node_modules/
            }
        ]
    },
    resolve: {
        extensions: ['.ts', '.js']
    },
    plugins: [
        new.CleanPlugin.CleanWebpackPlugin()
    ]
}
```
```json
{
    "build": "webpack --config webpack.config.prod.js"
}
```
