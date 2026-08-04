# Chapter 9: Generics in TypeScript

> Understanding how to create reusable, flexible, and type-safe components using Generics.

---

# Table of Contents

- [1. Introduction to Generics](#1-introduction-to-generics)
- [2. Why Do We Need Generics?](#2-why-do-we-need-generics)
- [3. Generic Syntax](#3-generic-syntax)
- [4. Generic Functions](#4-generic-functions)
- [5. Multiple Generic Parameters](#5-multiple-generic-parameters)
- [6. Generic Interfaces](#6-generic-interfaces)
- [7. Generic Classes](#7-generic-classes)
- [8. Generic Constraints](#8-generic-constraints)
- [9. Default Generic Types](#9-default-generic-types)
- [10. Real World Examples](#10-real-world-examples)
- [11. Generic vs any](#11-generic-vs-any)
- [12. Interview Questions](#12-interview-questions)
- [13. Key Takeaways](#13-key-takeaways)

---

# 1. Introduction to Generics

Generics allow us to create reusable code that works with different types while maintaining type safety.

Without Generics:

```typescript
function identity(value:any){

    return value;

}
```

Problem:

`any` removes TypeScript safety.

---

With Generics:

```typescript
function identity<T>(
    value:T
):T{

    return value;

}
```

Now TypeScript remembers the actual type.

---

# Generic Concept

Think of `T` as a placeholder.

```
Before execution:

T = unknown


When used:

T becomes the actual type
```

Example:

```
identity<number>(10)

T = number
```

```
identity<string>("Hello")

T = string
```

---

# 2. Why Do We Need Generics?

Consider:

```typescript
function getNumber(value:number):number{

    return value;

}
```

Only works with numbers.

---

For strings:

```typescript
function getString(value:string):string{

    return value;

}
```

Duplicate code.

---

Generics solve this:

```typescript
function getValue<T>(
value:T
):T{

    return value;

}
```

Works with:

```typescript
getValue<number>(10);

getValue<string>("Hello");
```

---

# 3. Generic Syntax

Syntax:

```typescript
function name<T>(
parameter:T
){

}
```

`T` is called:

```
Type Parameter
```

---

Example:

```typescript
function print<T>(
value:T
){

    console.log(value);

}
```

---

Calling:

```typescript
print<number>(100);

print<string>("TypeScript");
```

---

# 4. Generic Functions

## Basic Example

```typescript
function identity<T>(
value:T
):T{

    return value;

}
```

---

Usage:

```typescript
let num =
identity<number>(10);


let text =
identity<string>("Hello");
```

---

TypeScript can infer the type:

```typescript
let result =
identity(100);
```

TypeScript knows:

```
T = number
```

---

# Generic Array Function

Example:

```typescript
function getFirst<T>(
items:T[]
):T{

    return items[0];

}
```

Usage:

```typescript
getFirst<number>(
[1,2,3]
);
```

Returns:

```
number
```

---

```typescript
getFirst<string>(
["A","B"]
);
```

Returns:

```
string
```

---

# 5. Multiple Generic Parameters

A function can have multiple types.

Example:

```typescript
function pair<T,U>(
first:T,
second:U
){

    return {
        first,
        second
    };

}
```

Usage:

```typescript
pair<string,number>(
"Age",
25
);
```

Result:

```typescript
{
first:string;
second:number;
}
```

---

# 6. Generic Interfaces

Interfaces can also use Generics.

Example:

```typescript
interface Box<T>{

    value:T;

}
```

---

Using:

```typescript
const numberBox:
Box<number>
=
{
    value:100
};
```

---

String box:

```typescript
const stringBox:
Box<string>
=
{
    value:"Hello"
};
```

---

# Generic API Response Example

```typescript
interface ApiResponse<T>{

    data:T;

    status:number;

}
```

Usage:

```typescript
interface User{

    id:number;

    name:string;

}


const response:
ApiResponse<User>
=
{

data:{
    id:1,
    name:"Alex"
},

status:200

};
```

---

# 7. Generic Classes

Classes can also accept type parameters.

Example:

```typescript
class Storage<T>{

    value:T;


    constructor(value:T){

        this.value=value;

    }

}
```

---

Number storage:

```typescript
const numberStorage =
new Storage<number>(100);
```

---

String storage:

```typescript
const stringStorage =
new Storage<string>("Hello");
```

---

# Generic Stack Example

```typescript
class Stack<T>{

    items:T[]=[];


    push(item:T){

        this.items.push(item);

    }


    pop():T|undefined{

        return this.items.pop();

    }

}
```

Usage:

```typescript
const stack =
new Stack<number>();


stack.push(10);

stack.push(20);
```

---

# 8. Generic Constraints

Sometimes we need to restrict what types are allowed.

Keyword:

```
extends
```

---

Problem:

```typescript
function getLength<T>(
value:T
){

    return value.length;

}
```

Error:

```
Property length does not exist on type T
```

Because T can be:

```
number
boolean
object
```

---

Solution:

```typescript
function getLength<T extends {length:number}>(
value:T
){

    return value.length;

}
```

Now only types having:

```
length property
```

are allowed.

---

Valid:

```typescript
getLength("Hello");

getLength([1,2,3]);
```

Invalid:

```typescript
getLength(100);
```

---

# keyof Constraint

Example:

```typescript
function getProperty<
T,
K extends keyof T
>(
obj:T,
key:K
){

    return obj[key];

}
```

Usage:

```typescript
const user={

name:"Alex",

age:25

};


getProperty(
user,
"name"
);
```

---

# 9. Default Generic Types

Generics can have default values.

Syntax:

```typescript
<T = DefaultType>
```

---

Example:

```typescript
interface Response<T=string>{

    data:T;

}
```

---

Without specifying:

```typescript
const result:
Response
```

means:

```typescript
Response<string>
```

---

With custom type:

```typescript
Response<number>
```

---

# 10. Real World Examples

## API Client

```typescript
class ApiClient<T>{

    data:T;


    constructor(data:T){

        this.data=data;

    }

}
```

Usage:

```typescript
const users =
new ApiClient<User[]>([]);
```

---

# React Example

Generic components:

```typescript
function List<T>(
items:T[]
){

}
```

Used with:

```typescript
List<User>();

List<Product>();
```

---

# Database Repository

```typescript
class Repository<T>{

    find(id:number):T{

        return {} as T;

    }

}
```

Usage:

```typescript
const userRepo =
new Repository<User>();
```

---

# 11. Generic vs any

## any

```typescript
function test(
value:any
){

}
```

Problem:

Type checking disabled.

---

## Generic

```typescript
function test<T>(
value:T
){

}
```

Advantages:

- Keeps type information
- Provides autocomplete
- Maintains safety

---

Comparison:

| Feature | any | Generic |
|-|-|-|
| Type safety | ❌ | ✅ |
| Reusable | ✅ | ✅ |
| Autocomplete | ❌ | ✅ |
| Recommended | No | Yes |

---

# 12. Interview Questions

## Q1. What are Generics?

Answer:

Generics allow creating reusable components that work with different types while maintaining type safety.

---

## Q2. Why not use any?

Answer:

`any` removes type checking.

Generics preserve the actual type.

---

## Q3. What is T in Generics?

Answer:

`T` is a type parameter placeholder.

Example:

```typescript
<T>
```

---

## Q4. What are generic constraints?

Answer:

They restrict allowed types.

Example:

```typescript
<T extends object>
```

---

## Q5. Can classes use Generics?

Answer:

Yes.

Example:

```typescript
class Storage<T>
```

---

## Q6. What is keyof used with Generics?

Answer:

To restrict keys to existing properties.

Example:

```typescript
K extends keyof T
```

---

# 13. Key Takeaways

- Generics create reusable type-safe code.
- `T` represents a future type.
- Generic functions work with many types.
- Interfaces and classes can use Generics.
- Constraints restrict allowed types.
- Default generics provide fallback types.
- Generics are heavily used in:
  - React
  - Angular
  - NestJS
  - APIs
  - Libraries

---

# End of Chapter 9
