---
title: "Advance TypeScript Type Systems"
seoTitle: "Enhanced TypeScript Type Structures"
seoDescription: "Explore TypeScript's advanced features like union, intersection types, generics, and type inference for better coding and robust apps"
datePublished: Tue Aug 29 2023 13:27:21 GMT+0000 (Coordinated Universal Time)
cuid: cllwceupe000208lj1xpb0iks
slug: advance-typescript-type-systems
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/pP1f7Iwnd1Q/upload/f110faba201082dfe11a4799b90f363a.jpeg
tags: javascript, typescript

---

TypeScript has been the go-to solution for JavaScript developers seeking a more structured and reliable approach to coding in recent times. It has become a vital tool for many developers, enabling them to easily create robust and maintainable applications.

But TypeScript is more than just the basics. It has many advanced features that can improve your coding skills. In this article, we'll look at some complex parts of TypeScript's types, showing you the hidden tricks and how they can help you as a developer. We'll cover things like union and intersection types, generics, and type inference, so you can use TypeScript more effectively and accurately.

### Union Types

Think of unions as a type that combines two or more other types, representing values that may be *any one* of those types.

Consider, for instance, a function that can accept either a string or a number as its input. With a union type, you can precisely define this function's parameter, indicating that it expects either a string or a number, but nothing else. No more guessing games or runtime errors. TypeScript has got your back.

We create a union type with the pipe symbol `|` between the types you want to unify.

Look at this example

```typescript
const printUserId = (userId: number | string) => {
console.log('user id is ' + userId)
}

printUserId('100') // user id is 100 
printUserId(200)  //  user id is 200
```

In the example above, the `printUserId` function accepts a parameter of type `string | number`. It gracefully handles both strings and numbers while guarding against unintended inputs.

If we attempt to assign an incorrect type to our function parameter, TypeScript will give an error.

```typescript
printUserId(false) // Argument of type 'boolean' is not assignable to parameter of type 'string | number'
```

Let's go deeper into using unions if we try to use numbers or string methods within our function, typescript would allow us to do that cause it will only allow an operation if it is valid for every member of the union, and in this case, we have two member `number` `string`

```typescript
const printUserId = (userId: number | string) => {
console.log(userId.split()) //  Property 'split' does not exist on type 'number'
}
```

The code above will throw an error because we cannot use the `split` method on the number type. The split method turns a string into substrings and returns an array. So this is not possible with the number type we will get an error. We can solve this by narrowing the union. Narrowing happens when typescript deduces a more specific type for a value based on the structure of our code.

```typescript
const printUserId = (userId: number | string) => {
  if (typeof userId === 'string') {
    console.log(userId.split(","));
  } else {
    console.log(userId);
  }
};
```

For this solution typescript only knows a string value will have a `typeof` value "string" therefore the if statement and notice the else statement do not do much, if `userId` is not a string then it is a number.

### Intersection Types

We create an intersection type by combining multiple existing types and the new type will have all the features of the existing types.

Let's see an example, Imagine you're working on a project where you need to manage user roles and permissions and you have two types representing roles and permissions ;

```typescript
type UserRole = { role: string; };

type UserPermissions = { permissions: string[]; };
```

Now, let's imagine you want to represent a user who has not only a role but also a set of permissions. Intersection types can assist us in creating a new type that includes both aspects.

```typescript
type User = UserRole & UserPermissions;

const adminUser: User = { role: "admin", permissions: ["read", "write", "delete"], };
```

In this example, the `User` type is formed by combining the `UserRole` and `UserPermissions` types using the `&` symbol. This allows us to represent users with both a role and a set of permissions, providing a comprehensive view of their access rights.

### Generics

Now, let's discuss the concept of Generics in TypeScript. Generics are a powerful and flexible feature that allows developers to create reusable components that can work with different data types while maintaining type safety. By using Generics, you can write code that is adaptable to a wide range of types, without the need to explicitly specify the type in every instance.

Generics are particularly useful when working with data structures like arrays, objects, and classes, as they enable you to create components that can handle various data types without losing the benefits of type checking and code completion. This leads to cleaner, more maintainable code that is less prone to errors and can be easily extended to accommodate new requirements.

In TypeScript, you can define a generic type by using angle brackets (`<>`) and a type variable, which acts as a placeholder for the actual type that will be used when the generic component is instantiated. Let's see an example:

```typescript
const reverseArray = (arr: any[]): any[] => {
  return arr.reverse();
};

console.log(reverseArray([1, 2, 3, 4])); // [4, 3, 2, 1]
console.log(reverseArray(["hello", "humans"])); // ["humans", "hello"]
```

We define `reverseArray` a function that accepts an array of type `any` and returns the reversed values of the array. notice we used `any` type for our function, which means we can pass in `any` type of array to our function. in most cases, this might not be our desired behavior for the function cause we may want to reverse numbers of numbers or strings of string array and we don't want numbers in our string array or vice-versa.

Let's rewrite the same function above but this time around using generics.

```typescript
const reverseArray = <T>(arr: T[]): T[] => {
  return arr.reverse();
};

console.log(
  reverseArray<number>([1, 2, 3, 4])
);

console.log(
  reverseArray<string>(["hello", "humans"])
);
```

The type variable `T` is used to specify the type of the arguments, this means that the data type will be specified at the time of a function call and it also serves as the data type of the arguments.

So what this means is we call our generic function `reverseArray` and pass the numbers of arrays or the strings of arrays, we call the function as `reverseArray<number>([1, 2, 3, 4])` and replace `T` with numbers so the type of the arguments will always be an array of numbers and the same for the logic works `reverseArray<string>(["hello", "humans"])`

Generics provide a powerful way to create reusable, type-safe components that can adapt to different data types, ultimately leading to more robust and maintainable code.

### Type inference

What this means is typescript has the ability to determine the types of variables, parameters, and return values without explicit type annotations. This relieves us of the burden of constantly specifying types and allows typescript to do the heavy lifting.

```typescript
const message = "Hello, TypeScript"; 
const age  = 23
```

In the variable above TypeScript automatically infers the type of `message` and `age` based on its usage. If you later attempt to use `message` it in a way that doesn't align with its inferred type, TypeScript will alert you with an error message.

```typescript
age = message  // Type 'string' is not assignable to type 'number'
```

As demonstrated above, when we attempt to assign variable `age` to variable `message` , we do encounter an error. It's important to note that we didn't explicitly specify the types of our variables; TypeScript automatically handled that for us behind the scenes. So it inferred the type of string to variable `message` and type of number to variable `age .` That's cool right, type inference is helpful in type-checking when there are no explicit type annotations available.

### Conclusion

In conclusion, we have delved into a few key aspects of TypeScript types, which have provided us with a solid foundation for understanding the language. As a refresher, we first examined Union Types, which allow us to combine multiple types into one. Following that, we delved into Intersection Types, which enable us to merge multiple types into a single, more complex type. Additionally, we explored the concept of generics, which provide a way to create reusable components that can work with various types while maintaining type safety. Lastly, we touched upon the powerful feature of type inference, which allows TypeScript to automatically deduce the types of our variables when they are not explicitly specified, making type-checking more efficient and seamless.

Advanced topics and techniques are in abundance waiting to be discovered, which can further enhance our understanding and utilization of this versatile language. In the future, I will be delving into these advanced concepts and sharing my insights through a series of articles, so stay tuned for more exciting discoveries in the world of TypeScript.

Good luck ;)