# Chapter 15: Decorators (Introduction)

> Learn what decorators are, how they work, the different types of decorators, and why modern frameworks like NestJS and Angular depend on them.

---

# Table of Contents

- [1. What are Decorators?](#1-what-are-decorators)
- [2. Why Do We Need Decorators?](#2-why-do-we-need-decorators)
- [3. How Decorators Work](#3-how-decorators-work)
- [4. Enabling Decorators](#4-enabling-decorators)
- [5. Class Decorators](#5-class-decorators)
- [6. Method Decorators](#6-method-decorators)
- [7. Property Decorators](#7-property-decorators)
- [8. Parameter Decorators](#8-parameter-decorators)
- [9. Decorator Factories](#9-decorator-factories)
- [10. Decorator Metadata](#10-decorator-metadata)
- [11. Real World Examples](#11-real-world-examples)
- [12. Interview Questions](#12-interview-questions)
- [13. Key Takeaways](#13-key-takeaways)

---

# 1. What are Decorators?

A decorator is a special function that adds extra behavior to a class or one of its members **without modifying the original source code**.

Think of decorators as wrappers around existing code.

Example:

```typescript
class User {

}
```

Using a decorator:

```typescript
@Logger
class User {

}
```

Here,

```
@Logger
```

adds behavior to the class.

---

## Real-Life Analogy

Imagine a gift box.

```
Gift

↓

Wrap it

↓

Gift with decoration
```

The gift remains the same.

Only extra features are added.

Decorators work the same way.

---

# 2. Why Do We Need Decorators?

Without decorators:

```typescript
class User {

}

register(User);
validate(User);
log(User);
```

With decorators:

```typescript
@Register
@Validate
@Logger
class User {

}
```

Cleaner.

More readable.

Frameworks can discover information automatically.

---

# Common Uses

Decorators are used for:

- Dependency Injection
- Routing
- Validation
- Logging
- Authorization
- Caching
- Serialization
- ORM Mapping

Example in NestJS:

```typescript
@Controller("users")
class UserController {

}
```

---

# 3. How Decorators Work

A decorator is just a function.

Example:

```typescript
function Logger(
    target:Function
){

    console.log("Class Loaded");

}
```

Usage:

```typescript
@Logger
class User {

}
```

Equivalent to:

```typescript
User = Logger(User);
```

Conceptually:

```
Class

↓

Decorator

↓

Modified Class
```

---

# 4. Enabling Decorators

Decorators are **not enabled by default**.

Enable them in:

```json
tsconfig.json
```

```json
{
    "compilerOptions": {
        "experimentalDecorators": true
    }
}
```

If you want runtime metadata:

```json
{
    "compilerOptions": {
        "emitDecoratorMetadata": true
    }
}
```

Most frameworks also require:

```typescript
import "reflect-metadata";
```

---

# 5. Class Decorators

A class decorator receives the constructor.

Syntax:

```typescript
function Logger(
    constructor:Function
){

}
```

Example:

```typescript
function Logger(
    constructor:Function
){

    console.log(
        constructor.name
    );

}
```

Usage:

```typescript
@Logger
class User {

}
```

Output:

```
User
```

---

## Returning a New Class

A decorator may replace the original class.

```typescript
function Logger<T extends {
    new(...args:any[]):{}
}>(
    constructor:T
){

    return class extends constructor{

        createdAt = new Date();

    };

}
```

---

# 6. Method Decorators

Applied to methods.

Example:

```typescript
class User{

    @Log

    save(){

    }

}
```

Signature:

```typescript
function Log(

    target:Object,

    propertyKey:string,

    descriptor:PropertyDescriptor

){

}
```

Arguments:

| Parameter | Meaning |
|----------|---------|
| target | Prototype |
| propertyKey | Method name |
| descriptor | Method information |

---

## Example

```typescript
function Log(

    target:Object,

    propertyKey:string,

    descriptor:PropertyDescriptor

){

    console.log(propertyKey);

}
```

Output:

```
save
```

---

# 7. Property Decorators

Used on class properties.

Example:

```typescript
class User{

    @Required

    name!:string;

}
```

Signature:

```typescript
function Required(

    target:Object,

    propertyKey:string

){

}
```

Property decorators:

- Cannot change property values
- Mainly store metadata

---

# 8. Parameter Decorators

Applied to parameters.

Example:

```typescript
class User{

    save(

        @Inject id:number

    ){

    }

}
```

Signature:

```typescript
function Inject(

    target:Object,

    propertyKey:string,

    parameterIndex:number

){

}
```

Arguments:

```
Target

↓

Method

↓

Parameter Position
```

---

# 9. Decorator Factories

A decorator factory is a function that **returns** a decorator.

Instead of:

```typescript
@Logger
```

Use:

```typescript
@Logger()
```

Example:

```typescript
function Logger(){

    return function(

        constructor:Function

    ){

        console.log("Logging");

    };

}
```

Usage:

```typescript
@Logger()
class User{

}
```

---

## Decorator Factory with Arguments

```typescript
function Role(
    role:string
){

    return function(
        constructor:Function
    ){

        console.log(role);

    };

}
```

Usage:

```typescript
@Role("Admin")
class User{

}
```

Output:

```
Admin
```

---

# Why Frameworks Prefer @Decorator()

NestJS uses:

```typescript
@Controller("users")

@Injectable()

@Get(":id")
```

instead of:

```typescript
@Controller

@Injectable

@Get
```

Because factories allow configuration.

Example:

```typescript
@Controller("users")
```

Route path:

```
users
```

can be passed as an argument.

Without factories, there is no place to pass configuration.

---

# 10. Decorator Metadata

Metadata means:

> Information about code.

Example:

```typescript
class User{

    constructor(

        public name:string,

        public age:number

    ){

    }

}
```

With:

```json
"emitDecoratorMetadata": true
```

TypeScript emits metadata such as:

```
Parameter Types

↓

[String, Number]
```

Frameworks like NestJS use this for:

- Dependency Injection
- Validation
- Serialization

---

## reflect-metadata

Example:

```typescript
import "reflect-metadata";
```

Reading metadata:

```typescript
Reflect.getMetadata(
    "design:paramtypes",
    User
);
```

Returns:

```typescript
[
    String,
    Number
]
```

---

# 11. Real World Examples

## NestJS Controller

```typescript
@Controller("users")
class UserController{

}
```

---

## Route

```typescript
@Get(":id")
findOne(){

}
```

---

## Dependency Injection

```typescript
@Injectable()
class UserService{

}
```

---

## Validation

```typescript
class User{

    @IsEmail()

    email!:string;

}
```

---

# Decorator Execution Order

Given:

```typescript
@A
@B
class User{

}
```

Evaluation:

```
A()

↓

B()
```

Application:

```
B

↓

A
```

This is similar to nested function calls.

---

# 12. Interview Questions

## Q1. What is a decorator?

Answer:

A function that adds behavior to a class or its members without modifying the original code.

---

## Q2. What are the different types of decorators?

- Class Decorator
- Method Decorator
- Property Decorator
- Parameter Decorator

(TypeScript also supports Accessor Decorators, though they're not covered in this introductory chapter.)

---

## Q3. What is a decorator factory?

Answer:

A function that returns a decorator.

Example:

```typescript
@Logger()
```

---

## Q4. Difference between @Logger and @Logger()?

Answer:

`@Logger`

- Uses an existing decorator directly.

`@Logger()`

- Calls a function that returns a decorator.
- Allows passing configuration.

---

## Q5. Why does NestJS prefer decorator factories?

Answer:

Because decorators often need configuration.

Example:

```typescript
@Controller("users")
```

The `"users"` path is passed into the decorator factory.

---

## Q6. What is decorator metadata?

Answer:

Metadata is extra runtime information about classes, methods, and parameters that frameworks can inspect using `reflect-metadata`.

---

# 13. Key Takeaways

- Decorators add behavior without changing original code.
- Decorators are functions.
- Enable decorators using `experimentalDecorators`.
- Enable runtime metadata using `emitDecoratorMetadata`.
- Four common decorator types are:
  - Class
  - Method
  - Property
  - Parameter
- Decorator factories return decorators and accept configuration.
- Metadata enables Dependency Injection and reflection.
- Decorators are foundational concepts in NestJS, Angular, and many TypeScript libraries.

---

# End of Chapter 15
