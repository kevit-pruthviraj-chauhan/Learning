# Chapter 8: Union & Intersection Types in TypeScript

> Understanding how TypeScript combines types, restricts values, and safely works with multiple possible types.

---

# Table of Contents

- [1. Introduction](#1-introduction)
- [2. Union Types](#2-union-types)
- [3. Union Types with Functions](#3-union-types-with-functions)
- [4. Literal Types](#4-literal-types)
- [5. Intersection Types](#5-intersection-types)
- [6. Type Narrowing](#6-type-narrowing)
- [7. Type Guards](#7-type-guards)
  - typeof Guard
  - instanceof Guard
  - in Operator Guard
  - Equality Narrowing
  - Custom Type Guards
- [8. Discriminated Unions](#8-discriminated-unions)
- [9. Real World Examples](#9-real-world-examples)
- [10. Union vs Intersection](#10-union-vs-intersection)
- [11. Interview Questions](#11-interview-questions)
- [12. Key Takeaways](#12-key-takeaways)

---

# 1. Introduction

In TypeScript, sometimes a value can have multiple possible forms.

Examples:

A user ID:

```
number
```

or:

```
string
```

An API response:

```
Success Response
```

or:

```
Error Response
```

TypeScript provides:

- Union Types
- Intersection Types
- Literal Types
- Type Narrowing
- Type Guards

to handle these situations safely.

---

# 2. Union Types

## Definition

A union type allows a variable to store more than one type.

Symbol:

```
|
```

Meaning:

```
OR
```

---

Example:

```typescript
let id:string | number;
```

This means:

```
id can be:

string

OR

number
```

---

Valid:

```typescript
id = "user101";

id = 101;
```

Invalid:

```typescript
id = true;
```

Error:

```
Type 'boolean' is not assignable
```

---

# Union Type Diagram

```
          string

             \
              \
               ---> value
              /
             /

          number
```

A union value can belong to any one of the types.

---

# 3. Union Types with Functions

Example:

```typescript
function printId(
    id:string | number
){

    console.log(id);

}
```

Usage:

```typescript
printId(100);

printId("ABC100");
```

Both are valid.

---

## Problem with Union Types

Example:

```typescript
function getLength(
    value:string | number
){

    return value.length;

}
```

Error:

```
Property 'length' does not exist on type 'number'
```

Why?

Because:

```
value can be:

string → has length

number → no length
```

TypeScript prevents unsafe operations.

---

# 4. Literal Types

## Definition

Literal types allow only specific values.

Example:

```typescript
let direction:
"left" | "right";
```

Allowed:

```typescript
direction="left";

direction="right";
```

Not allowed:

```typescript
direction="up";
```

---

# Literal Type Example

Without literal types:

```typescript
let status:string;
```

All values allowed:

```
success
error
loading
random
```

---

With literal types:

```typescript
type Status =
"success" |
"error" |
"loading";
```

Only:

```
success
error
loading
```

are accepted.

---

# Real World Example

API status:

```typescript
type ApiStatus =
"pending" |
"success" |
"failed";
```

Usage:

```typescript
let status:ApiStatus;

status="success";
```

---

# 5. Intersection Types

## Definition

Intersection combines multiple types into one.

Symbol:

```
&
```

Meaning:

```
AND
```

---

Example:

```typescript
type User = {

    name:string;

};


type Employee = {

    employeeId:number;

};


type Staff = User & Employee;
```

Now Staff requires:

```
name

AND

employeeId
```

---

Usage:

```typescript
const staff:Staff = {

    name:"Alex",

    employeeId:101

};
```

---

# Intersection Diagram

```
User

name:string


        +

        &


Employee

employeeId:number


        |

        v


Staff

name:string
employeeId:number
```

---

# Multiple Intersections

Example:

```typescript
type Admin =
User &
Permission &
Settings;
```

Admin contains:

```
User properties

+

Permission properties

+

Settings properties
```

---

# 6. Type Narrowing

## Definition

Type narrowing means reducing a broad type into a more specific type.

Example:

Before:

```typescript
string | number
```

After checking:

```typescript
string
```

---

Example:

```typescript
function process(
value:string | number
){

    if(typeof value==="string"){

        value.toUpperCase();

    }

}
```

Inside the `if` block:

TypeScript knows:

```
value = string
```

---

# Why Type Narrowing is Required

Example:

```typescript
function calculate(
value:string | number
){

    value.toFixed();

}
```

Error:

```
Property 'toFixed'
does not exist on type 'string'
```

Because value might be:

```
string
```

---

# 7. Type Guards

Type guards are techniques that help TypeScript identify the exact type.

Main type guards:

1. typeof
2. instanceof
3. in
4. equality checks
5. custom guards

---

# 7.1 typeof Guard

Used for primitive types.

Example:

```typescript
function format(
value:string | number
){

    if(typeof value==="string"){

        return value.toUpperCase();

    }


    return value.toFixed();

}
```

TypeScript understands:

```
if block:

string


else:

number
```

---

# 7.2 instanceof Guard

Used with classes.

Example:

```typescript
class Dog {

    bark(){

        console.log("Bark");

    }

}


class Cat {

    meow(){

        console.log("Meow");

    }

}


function makeSound(
animal:Dog | Cat
){

    if(animal instanceof Dog){

        animal.bark();

    }

}
```

---

# 7.3 in Operator Guard

Checks whether a property exists.

Example:

```typescript
type Car = {

    drive():void;

};


type Boat = {

    sail():void;

};


function move(
vehicle:Car | Boat
){

    if("drive" in vehicle){

        vehicle.drive();

    }

}
```

---

# 7.4 Equality Narrowing

Example:

```typescript
function compare(
a:string|number,
b:string|number
){

    if(a===b){

        console.log(a);

    }

}
```

After comparison:

TypeScript knows:

```
a and b are same type
```

---

# 7.5 Custom Type Guards

We can create our own type guards.

Syntax:

```typescript
parameter is Type
```

Example:

```typescript
type User = {

    name:string;

};


function isUser(
value:any
):value is User{

    return value.name !== undefined;

}
```

Usage:

```typescript
if(isUser(data)){

    console.log(data.name);

}
```

---

# 8. Discriminated Unions

A powerful TypeScript pattern.

Uses a common property to identify types.

Example:

```typescript
type Success = {

    status:"success";

    data:string;

};


type ErrorResponse = {

    status:"error";

    message:string;

};


type Response =
Success | ErrorResponse;
```

---

Usage:

```typescript
function handle(
response:Response
){

    if(response.status==="success"){

        console.log(response.data);

    }

    else{

        console.log(response.message);

    }

}
```

TypeScript automatically narrows the type.

---

# 9. Real World Examples

## User Role System

```typescript
type Role =
"admin" |
"editor" |
"user";
```

---

## API Response

```typescript
type ApiResponse =
{
    success:true;
    data:string;
}
|
{
    success:false;
    error:string;
};
```

---

## Combining Objects

```typescript
type Person = {

    name:string;

};


type Developer = {

    language:string;

};


type Programmer =
Person & Developer;
```

---

# 10. Union vs Intersection

| Feature | Union | Intersection |
|---|---|---|
| Symbol | `|` | `&` |
| Meaning | OR | AND |
| Values | One of many types | Combination of types |
| Example | string \| number | User & Employee |

---

Union:

```
A OR B
```

Example:

```typescript
string | number
```

---

Intersection:

```
A AND B
```

Example:

```typescript
User & Admin
```

---

# 11. Interview Questions

## Q1. What is a union type?

Answer:

A type that allows multiple possible types.

Example:

```typescript
string | number
```

---

## Q2. What is an intersection type?

Answer:

A type that combines multiple types.

Example:

```typescript
User & Employee
```

---

## Q3. Difference between union and intersection?

Answer:

Union:

```
OR
```

Intersection:

```
AND
```

---

## Q4. What is type narrowing?

Answer:

Converting a broad type into a specific type using checks.

---

## Q5. What are type guards?

Answer:

Techniques that help TypeScript determine the actual type.

Examples:

- typeof
- instanceof
- in

---

## Q6. Why are literal types useful?

Answer:

They restrict values to predefined choices.

Example:

```typescript
"success"|"failed"
```

---

# 12. Key Takeaways

- Union types allow multiple possible types.
- Intersection types combine multiple types.
- `|` means OR.
- `&` means AND.
- Literal types restrict exact values.
- Type narrowing makes union types safe.
- Type guards help TypeScript understand types.
- Discriminated unions are powerful for API states.
- These concepts are heavily used in professional TypeScript projects.

---

# End of Chapter 8
