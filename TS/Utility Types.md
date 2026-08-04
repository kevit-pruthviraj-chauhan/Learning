# Chapter 7: Utility Types in TypeScript

> Utility Types are built-in TypeScript tools that help transform existing types into new types without rewriting them.

---

# Table of Contents

- [1. Introduction](#1-introduction)
- [2. Partial](#2-partial)
- [3. Required](#3-required)
- [4. Readonly](#4-readonly)
- [5. Pick](#5-pick)
- [6. Omit](#6-omit)
- [7. Record](#7-record)
- [8. Exclude](#8-exclude)
- [9. Extract](#9-extract)
- [10. NonNullable](#10-nonnullable)
- [11. ReturnType](#11-returntype)
- [12. Parameters](#12-parameters)
- [13. Utility Types Summary](#13-utility-types-summary)
- [14. Interview Questions](#14-interview-questions)
- [15. Key Takeaways](#15-key-takeaways)

---

# 1. Introduction

In real projects, we often need variations of existing types.

Example:

Original type:

```typescript
type User = {

    id:number;

    name:string;

    email:string;

};
```

Now we need:

- User update object
- User response object
- Readonly user
- User without password

Instead of creating new types manually, TypeScript provides Utility Types.

---

# 2. Partial

## Meaning

> Make all properties optional.

Syntax:

```typescript
Partial<Type>
```

---

Example:

```typescript
type User = {

    id:number;

    name:string;

    email:string;

};
```

Using:

```typescript
type UpdateUser = Partial<User>;
```

Becomes:

```typescript
type UpdateUser = {

    id?:number;

    name?:string;

    email?:string;

};
```

---

Usage:

```typescript
const update:UpdateUser = {

    name:"Alex"

};
```

Useful for:

- Update APIs
- PATCH requests
- Forms

---

# 3. Required

## Meaning

> Make all optional properties required.

Syntax:

```typescript
Required<Type>
```

---

Example:

```typescript
type User = {

    name?:string;

    age?:number;

};
```

Using:

```typescript
type CompleteUser = Required<User>;
```

Result:

```typescript
type CompleteUser = {

    name:string;

    age:number;

};
```

---

# 4. Readonly

## Meaning

> Prevent modification of properties.

Syntax:

```typescript
Readonly<Type>
```

---

Example:

```typescript
type User = {

    id:number;

    name:string;

};
```

Using:

```typescript
type ReadonlyUser = Readonly<User>;
```

Now:

```typescript
const user:ReadonlyUser = {

    id:1,

    name:"Alex"

};
```

Cannot:

```typescript
user.id=2;
```

Error:

```
Cannot assign to 'id'
```

---

Useful for:

- Configuration objects
- Constants
- Immutable data

---

# 5. Pick

## Meaning

> Select specific properties from a type.

Syntax:

```typescript
Pick<Type, Keys>
```

---

Example:

```typescript
type User = {

    id:number;

    name:string;

    email:string;

    password:string;

};
```

Need only:

```
id
name
```

Use:

```typescript
type UserPreview =
Pick<User,"id"|"name">;
```

Result:

```typescript
type UserPreview = {

    id:number;

    name:string;

};
```

---

Useful for:

- API responses
- Smaller objects

---

# 6. Omit

## Meaning

> Remove specific properties from a type.

Syntax:

```typescript
Omit<Type, Keys>
```

---

Example:

```typescript
type User = {

    id:number;

    name:string;

    password:string;

};
```

Remove password:

```typescript
type PublicUser =
Omit<User,"password">;
```

Result:

```typescript
type PublicUser = {

    id:number;

    name:string;

};
```

---

Common use:

Never send:

```
password
token
internal fields
```

to frontend.

---

# 7. Record

## Meaning

> Create an object type with specific keys and values.

Syntax:

```typescript
Record<Key,Value>
```

---

Example:

```typescript
type Roles =
Record<string,string>;
```

Valid:

```typescript
const roles = {

    admin:"ADMIN",

    user:"USER"

};
```

---

Another example:

```typescript
type UserRoles =
Record<
"admin"|"user",
boolean
>;
```

Result:

```typescript
{

admin:boolean;

user:boolean;

}
```

---

Useful for:

- Dictionaries
- Maps
- Configuration objects

---

# 8. Exclude

## Meaning

> Remove members from a union type.

Syntax:

```typescript
Exclude<Type,ExcludedMembers>
```

---

Example:

```typescript
type Status =
"success"|
"error"|
"loading";
```

Remove loading:

```typescript
type ApiStatus =
Exclude<Status,"loading">;
```

Result:

```typescript
"success"|"error"
```

---

# 9. Extract

## Meaning

> Keep only matching union members.

Syntax:

```typescript
Extract<Type,Union>
```

---

Example:

```typescript
type Status =
"success"|
"error"|
"loading";
```

Keep only:

```typescript
type SuccessStatus =
Extract<
Status,
"success"
>;
```

Result:

```typescript
"success"
```

---

Difference:

Exclude:

```
Remove
```

Extract:

```
Keep
```

---

# 10. NonNullable

## Meaning

> Remove null and undefined from a type.

Syntax:

```typescript
NonNullable<Type>
```

---

Example:

```typescript
type User =
string |
null |
undefined;
```

Using:

```typescript
type ValidUser =
NonNullable<User>;
```

Result:

```typescript
string
```

---

Useful with:

- API responses
- Optional values

---

# 11. ReturnType

## Meaning

> Get the return type of a function.

Syntax:

```typescript
ReturnType<Function>
```

---

Example:

```typescript
function getUser(){

    return {

        id:1,

        name:"Alex"

    };

}
```

Get return type:

```typescript
type User =
ReturnType<typeof getUser>;
```

Result:

```typescript
{

id:number;

name:string;

}
```

---

Useful when:

- Reusing function output types
- Avoiding duplicate types

---

# 12. Parameters

## Meaning

> Get function parameter types as a tuple.

Syntax:

```typescript
Parameters<Function>
```

---

Example:

```typescript
function login(
username:string,
password:string
){

}
```

Using:

```typescript
type LoginParams =
Parameters<typeof login>;
```

Result:

```typescript
[
string,
string
]
```

---

# 13. Utility Types Summary

| Utility Type | Meaning |
|-|-|
| Partial | Make everything optional |
| Required | Make everything required |
| Readonly | Prevent modification |
| Pick | Select properties |
| Omit | Remove properties |
| Record | Create key-value objects |
| Exclude | Remove union members |
| Extract | Keep union members |
| NonNullable | Remove null/undefined |
| ReturnType | Get function output type |
| Parameters | Get function arguments |

---

# 14. Interview Questions

## Q1. Difference between Pick and Omit?

Answer:

Pick:

```
Choose properties
```

Omit:

```
Remove properties
```

Example:

```typescript
Pick<User,"name">
```

keeps name.

```typescript
Omit<User,"password">
```

removes password.

---

## Q2. Difference between Partial and Required?

Answer:

Partial:

```
Everything optional
```

Required:

```
Everything mandatory
```

---

## Q3. Why use ReturnType?

Answer:

To reuse a function's return type without manually creating another type.

---

## Q4. Difference between Exclude and Extract?

Answer:

Exclude:

```
Remove matching types
```

Extract:

```
Keep matching types
```

---

# 15. Key Takeaways

- Utility types transform existing types.
- They reduce duplicate code.
- Partial is useful for updates.
- Pick and Omit are useful for API models.
- Readonly creates immutable objects.
- Record creates dictionary-like objects.
- Exclude and Extract work with unions.
- ReturnType and Parameters extract function information.
- Utility types are heavily used in professional TypeScript projects.

---

# End of Chapter 7
