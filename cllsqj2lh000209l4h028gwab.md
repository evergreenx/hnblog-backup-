---
title: "Zod and Yup"
seoTitle: "Zod Yup"
seoDescription: "Compare Zod & Yup for use in your project"
datePublished: Sat Jul 15 2023 23:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cllsqj2lh000209l4h028gwab
slug: zod-and-yup
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/WDbuusPOnkM/upload/c9b6938e966975102a8e9752e4a0e8a1.jpeg
tags: yup, zod

---

Let's compare Zod and Yup. Here are their respective definitions based on the documentation:

Zod: is a TypeScript-first schema declaration and validation library.

Yup: is a schema builder for runtime value parsing and validation.

Let's delve deeper into each of these powerful libraries, to gain a better understanding of their capabilities we will be looking at how they can be effectively used for validating critical credit card payment information such as card numbers, expiration dates, and CVV codes. By ensuring that these crucial data points meet the specified criteria, we can enhance the security and reliability of a payment processing system.

## Zod

Zod is a TypeScript-focused library designed for data validation. It enables you to establish data schemas as TypeScript types, which are subsequently utilized for validation purposes. Additionally, Zod performs runtime validation to ensure that data complies with the specified schema. By leveraging TypeScript's type inference, it provides robust type checking during development.

Check the documentation for instructions on setting up [Zod](https://zod.dev/?id=installation) in your application.

See codesandbox below

%[https://codesandbox.io/s/zod-and-yup-33mmq3?from-embed=&file=/src/index.ts] 

Here, we define a `paymentSchema` using Zod. It's structured as an object, where each field (e.g., `cardNumber`, `expirationDate`, `cvv`) is associated with specific validation rules.

For the `cardNumber`, it expects a string with a minimum length of 16 characters and a maximum length of 16 characters, indicating a typical credit card number.

The `expirationDate` field is validated using a regular expression (`regex`). It checks if the input matches the format "MM/YY," where MM is the month and YY is the year.

The `cvv` field expects a string with a minimum length of 3 characters and a maximum length of 4 characters, which is common for CVV codes

Next, we created a sample `paymentData` object with values for `cardNumber`, `expirationDate`, and `cvv`. These values will be validated against the rules defined in the schema.

The code snippet is wrapped in a `try...catch` block to handle potential validation errors.

Within the try block, the `paymentSchema.parse(paymentData)` line validates the `paymentData` object against the defined schema.

If the data matches the schema rules, it's considered valid, and the `validatedPayment` object is logged into the console, indicating successful validation.

If any validation rule is violated, an error is caught in the catch block. The error message is extracted and logged to the console, providing information about the validation issue.

## Yup

Yup is a widely used library for schema-based validation, frequently employed in React applications, particularly for form validation. It provides a declarative API for creating validation schemas through method chaining, simplifying the expression of validation rules. Yup accommodates asynchronous proof, which proves beneficial in situations such as API calls during the validation process.

For installation steps, please refer to the [documentation](https://github.com/jquense/yup).

Let's do same example as before but this around we will be using Yup

%[https://codesandbox.io/s/zod-and-yup-33mmq3?from-embed=&file=/src/index.ts] 

Same as the zod code. we defined a `paymentSchema` using Yup. It's structured as an object and includes validation rules for specific fields (`cardNumber`, `expirationDate`, and `cvv`).

For `cardNumber`, we expect a string of exactly 16 characters, which is a typical length for credit card numbers.

The `expirationDate` field is validated using a regular expression (`regex`). It checks if the input matches the format "MM/YY," where MM represents the month and YY the year.

The `cvv` field expects a string with a minimum length of 3 characters and a maximum length of 4 characters, typical for CVV codes.

Next, we create a sample `paymentData` object, populated with values for `cardNumber`, `expirationDate`, and `cvv`. These values will be validated against the rules defined in the schema.

`paymentSchema.validate(paymentData)` is invoked, which validates the `paymentData` object against the defined schema.

If the data matches the schema rules, it's considered valid, and the `validatedPayment` object is logged to the console, indicating successful validation.

If any validation rule is violated, the error is caught in the `.catch` block, and the error message is extracted and logged to the console, providing insights into the validation issue.

## **Conclusion**

The key takeaway is that when choosing between Zod and Yup for data validation, you should consider your project. Zod is a more general-purpose library with strong TypeScript integration, suitable for various data validation needs. On the other hand, Yup is well-suited for form validation in applications, thanks to its declarative API and integration with popular front-end libraries.