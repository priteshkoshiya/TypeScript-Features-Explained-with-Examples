# TypeScript Features Explained with Examples

## Table of Contents
1. Basic Types
2. Interfaces
3. Classes
4. Functions
5. Generics
6. Enums
7. Advanced Types
8. Decorators
9. Modules and Namespaces
10. Type Manipulation

## 1. Basic Types

```typescript
// Boolean
let isDone: boolean = false;

// Number
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;

// String
let color: string = "blue";
let sentence: string = `The color is ${color}`;

// Array
let list: number[] = [1, 2, 3];
let genericList: Array<number> = [1, 2, 3];

// Tuple
let x: [string, number] = ["hello", 10];

// Enum
enum Color {
    Red,
    Green,
    Blue
}
let c: Color = Color.Green;

// Any
let notSure: any = 4;
notSure = "maybe a string";
notSure = false;

// Void
function warnUser(): void {
    console.log("This is a warning message");
}

// Null and Undefined
let u: undefined = undefined;
let n: null = null;

// Never
function error(message: string): never {
    throw new Error(message);
}

// Object
let obj: object = { key: "value" };
```

## 2. Interfaces

```typescript
// Basic Interface
interface User {
    name: string;
    age: number;
    email?: string; // Optional property
    readonly id: number; // Read-only property
}

let user: User = {
    name: "John",
    age: 30,
    id: 1
};

// Function Interface
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc = function(src: string, sub: string): boolean {
    return src.search(sub) > -1;
};

// Class Interface
interface ClockInterface {
    currentTime: Date;
    setTime(d: Date): void;
}

class Clock implements ClockInterface {
    currentTime: Date = new Date();
    setTime(d: Date) {
        this.currentTime = d;
    }
}

// Extending Interfaces
interface Shape {
    color: string;
}

interface Square extends Shape {
    sideLength: number;
}

let square: Square = {
    color: "blue",
    sideLength: 10
};
```

## 3. Classes

```typescript
class Animal {
    private name: string;
    protected age: number;
    readonly species: string;

    constructor(name: string, age: number, species: string) {
        this.name = name;
        this.age = age;
        this.species = species;
    }

    public makeSound(): void {
        console.log("Some sound");
    }
}

// Inheritance
class Dog extends Animal {
    constructor(name: string, age: number) {
        super(name, age, "Canis");
    }

    public makeSound(): void {
        console.log("Woof!");
    }

    public getAge(): number {
        return this.age; // Can access protected member
    }
}

// Abstract Class
abstract class Department {
    constructor(public name: string) {}

    abstract printMeeting(): void;
}

class AccountingDepartment extends Department {
    printMeeting(): void {
        console.log("Accounting meeting");
    }
}
```

## 4. Functions

```typescript
// Function Type
let myAdd: (x: number, y: number) => number = 
    function(x: number, y: number): number { return x + y; };

// Optional and Default Parameters
function buildName(firstName: string, lastName?: string): string {
    return lastName ? `${firstName} ${lastName}` : firstName;
}

function greet(name: string = "Anonymous"): string {
    return `Hello, ${name}!`;
}

// Rest Parameters
function sum(...numbers: number[]): number {
    return numbers.reduce((total, num) => total + num, 0);
}

// This and Arrow Functions
interface Card {
    suit: string;
    card: number;
}

interface Deck {
    suits: string[];
    cards: number[];
    createCardPicker(this: Deck): () => Card;
}

let deck: Deck = {
    suits: ["hearts", "spades", "clubs", "diamonds"],
    cards: Array(52),
    createCardPicker(this: Deck) {
        return () => {
            let pickedCard = Math.floor(Math.random() * 52);
            let pickedSuit = Math.floor(pickedCard / 13);
            return {suit: this.suits[pickedSuit], card: pickedCard % 13};
        }
    }
};
```

## 5. Generics

```typescript
// Generic Function
function identity<T>(arg: T): T {
    return arg;
}

let output1 = identity<string>("myString");
let output2 = identity(42); // Type inference

// Generic Interface
interface GenericIdentityFn<T> {
    (arg: T): T;
}

let myIdentity: GenericIdentityFn<number> = identity;

// Generic Class
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };

// Generic Constraints
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
}

// Using Type Parameters in Generic Constraints
function getProperty<T, K extends keyof T>(obj: T, key: K) {
    return obj[key];
}

let x = { a: 1, b: 2, c: 3 };
getProperty(x, "a"); // okay
// getProperty(x, "d"); // error: Argument of type 'd' isn't assignable to 'a' | 'b' | 'c'
```

## 6. Enums

```typescript
// Numeric Enum
enum Direction {
    Up = 1,
    Down,
    Left,
    Right
}

// String Enum
enum DirectionString {
    Up = "UP",
    Down = "DOWN",
    Left = "LEFT",
    Right = "RIGHT"
}

// Heterogeneous Enum
enum BooleanLikeHeterogeneousEnum {
    No = 0,
    Yes = "YES",
}

// Computed and Constant Members
enum FileAccess {
    None,
    Read = 1 << 1,
    Write = 1 << 2,
    ReadWrite = Read | Write,
    G = "123".length
}

// Reverse Mapping
enum Enum {
    A
}
let a = Enum.A;
let nameOfA = Enum[a]; // "A"
```

## 7. Advanced Types

```typescript
// Union Types
function padLeft(value: string, padding: string | number) {
    // ...
}

// Type Guards
function isString(x: any): x is string {
    return typeof x === "string";
}

// Intersection Types
interface ErrorHandling {
    success: boolean;
    error?: { message: string };
}

interface ArtworksData {
    artworks: { title: string }[];
}

type ArtworksResponse = ArtworksData & ErrorHandling;

// Type Aliases
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;

// Literal Types
type Easing = "ease-in" | "ease-out" | "ease-in-out";

// Discriminated Unions
interface Square {
    kind: "square";
    size: number;
}
interface Rectangle {
    kind: "rectangle";
    width: number;
    height: number;
}
type Shape = Square | Rectangle;

// Index Types
function pluck<T, K extends keyof T>(o: T, propertyNames: K[]): T[K][] {
  return propertyNames.map(n => o[n]);
}
```

## 8. Decorators

```typescript
// Class Decorator
function reportableClass<T extends { new (...args: any[]): {} }>(constructor: T) {
    return class extends constructor {
        reportingURL = "http://www.example.com";
    };
}

@reportableClass
class BugReport {
    type = "report";
    title: string;

    constructor(t: string) {
        this.title = t;
    }
}

// Method Decorator
function enumerable(value: boolean) {
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        descriptor.enumerable = value;
    };
}

class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }

    @enumerable(false)
    greet() {
        return "Hello, " + this.greeting;
    }
}

// Property Decorator
function format(formatString: string) {
    return function (target: any, propertyKey: string): void {
        let value = target[propertyKey];

        const getter = function() {
            return `${formatString} ${value}`;
        };

        const setter = function(newVal: string) {
            value = newVal;
        };

        Object.defineProperty(target, propertyKey, {
            get: getter,
            set: setter,
            enumerable: true,
            configurable: true
        });
    };
}
```

## 9. Modules and Namespaces

```typescript
// Modules
// math.ts
export function add(x: number, y: number): number {
    return x + y;
}

export function subtract(x: number, y: number): number {
    return x - y;
}

// main.ts
import { add, subtract } from "./math";
let result = add(1, 2);

// Default Export
// jquery.d.ts
declare let $: JQuery;
export default $;

// App.ts
import $ from "jquery";

// Namespaces
namespace Validation {
    export interface StringValidator {
        isAcceptable(s: string): boolean;
    }

    const lettersRegexp = /^[A-Za-z]+$/;
    const numberRegexp = /^[0-9]+$/;

    export class LettersOnlyValidator implements StringValidator {
        isAcceptable(s: string) {
            return lettersRegexp.test(s);
        }
    }
}
```

## 10. Type Manipulation

```typescript
// Mapped Types
type ReadOnly<T> = {
    readonly [P in keyof T]: T[P];
}

interface Todo {
    title: string;
    description: string;
}

const myTodo: ReadOnly<Todo> = {
    title: "Delete inactive users",
    description: "..."
};

// Conditional Types
type TypeName<T> =
    T extends string ? "string" :
    T extends number ? "number" :
    T extends boolean ? "boolean" :
    T extends undefined ? "undefined" :
    T extends Function ? "function" :
    "object";

type T0 = TypeName<string>;  // "string"
type T1 = TypeName<"a">;     // "string"
type T2 = TypeName<true>;    // "boolean"

// Utility Types
interface Todo {
    title: string;
    description: string;
    completed: boolean;
}

type TodoPreview = Pick<Todo, "title" | "completed">;
type TodoInfo = Omit<Todo, "completed">;

// Template Literal Types
type World = "world";
type Greeting = `hello ${World}`;

type EmailLocaleIDs = "welcome_email" | "email_heading";
type FooterLocaleIDs = "footer_title" | "footer_sendoff";
type AllLocaleIDs = `${EmailLocaleIDs | FooterLocaleIDs}_id`;
```

## Best Practices

1. **Use `interface` for object types**
```typescript
// Good
interface User {
    name: string;
    age: number;
}

// Avoid
type User = {
    name: string;
    age: number;
};
```

2. **Use Type Guards**
```typescript
function processValue(value: string | number) {
    if (typeof value === "string") {
        // TypeScript knows value is string here
        return value.toUpperCase();
    }
    // TypeScript knows value is number here
    return value.toFixed(2);
}
```

3. **Leverage TypeScript's Type Inference**
```typescript
// Good
const numbers = [1, 2, 3]; // Type is inferred as number[]

// Unnecessary
const numbers: number[] = [1, 2, 3];
```

## Debugging Tips

1. **Using `keyof` for Debugging**
```typescript
type Keys = keyof Todo; // "title" | "description" | "completed"
```

2. **Type Assertion for Edge Cases**
```typescript
interface Person {
    name: string;
    age: number;
}

const person = {} as Person;
person.name = "John";
person.age = 30;
```

This comprehensive guide covers the core features of TypeScript with practical examples. Is there any specific aspect you'd like me to elaborate on further?
