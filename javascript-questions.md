


| Expression    | Result            | Reason geeksforgeeks​                         |
| ------------- | ----------------- | --------------------------------------------- |
| 1 + "2" + 3   | "123"             | 1 + "2" → "12", then "12" + 3 → "123"         |
| 1 + 2 + "3"   | "33"              | 1 + 2 → 3, then 3 + "3" → "33"                |
| "1" + 2 + 3   | "123"             | Both additions concatenate after first string |
| true + 1      | 2                 | true → 1 (boolean coercion), then 1 + 1       |
| null + 1      | 1                 | null → 0, then 0 + 1                          |
| undefined + 1 | "1undefined"      | undefined → "undefined", then concatenate     |
| [] + {}       | "[object Object]" | [] → "", {} → "[object Object]"               |
| {} + []       | "[object Object]" | {} → "[object Object]", [] → ""               |



| Question           | Result | Reason                                           |
| ------------------ | ------ | ------------------------------------------------ |
| +0 === -0          | true   | Bitwise identical in JS telerik​                 |
| null === 0         | false  | Different types geeksforgeeks​                   |
| undefined === null | false  | Different types hellojavascript​                 |
| [] === 0           | false  | Object vs number developer.mozilla​              |
| {} === {}          | false  | Different object references developer.mozilla+1​ |



| Expression                         | Result | Reason                                                                               |
| ---------------------------------- | ------ | ------------------------------------------------------------------------------------ |
| +0 == -0                           | true   | Both are numeric zero; == and === treat them as the same value. developer.mozilla+1​ |
| Object.is(+0, -0)                  | false  | Object.is distinguishes the sign of zero. stackoverflow+1​                           |
| NaN == NaN                         | false  | NaN is never equal to itself for == or ===. developer.mozilla+1​                     |
| Object.is(NaN, NaN)                | true   | Object.is has a special rule that treats NaN as equal to NaN. stackoverflow+1​       |
| null == undefined                  | true   | Special loose‑equality rule: only equal to each other. developer.mozilla+1​          |
| null == 0                          | false  | == does not coerce null to number. stackoverflow+1​                                  |
| null >= 0                          | true   | Relational >= converts null to 0, so 0 >= 0. stackoverflow+1​                        |
| null > 0                           | false  | null → 0; 0 > 0 is false. stackoverflow​                                             |
| 0 == undefined                     | false  | No loose‑equality rule between number and undefined. developer.mozilla+1​            |
| true == 1                          | true   | true → 1; compare 1 == 1. developer.mozilla+1​                                       |
| false == 0                         | true   | false → 0; compare 0 == 0. developer.mozilla+1​                                      |
| true == "1"                        | true   | true → 1, "1" → 1; compare 1 == 1. developer.mozilla+1​                              |
| false == ""                        | true   | false → 0, "" → 0; compare 0 == 0. developer.mozilla+1​                              |
| false == "0"                       | true   | false → 0, "0" → 0. developer.mozilla+1​                                             |
| "5" == 5                           | true   | "5" → number 5. developer.mozilla+1​                                                 |
| "" == 0                            | true   | "" → 0 in numeric coercion. stackoverflow+1​                                         |
| "  \\n\\t " == 0                   | true   | Whitespace string trims to "" then → 0. javascript+1​                                |
| "0" == 0                           | true   | "0" → 0. developer.mozilla+1​                                                        |
| [] == ""                           | true   | [] → "" via toString. developer.mozilla+1​                                           |
| [] == 0                            | true   | [] → "" → 0. developer.mozilla+1​                                                    |
| ["1"] == 1                         | true   | ["1"] → "1" → 1. developer.mozilla+1​                                                |
| [1,2,3] == "1,2,3"                 | true   | Array stringifies to "1,2,3". developer.mozilla​                                     |
| [] == false                        | true   | [] → "" → 0; false → 0. dev+1​                                                       |
| ![] == []                          | true   | ![] → false → 0; [] → "" → 0. dev​                                                   |
| { } == { }                         | false  | Different object references. developer.mozilla+1​                                    |
| new String("x") == "x"             | true   | Wrapper object → primitive "x". developer.mozilla​                                   |
| new String("x") == new String("x") | false  | Two distinct objects, no coercion to same reference. developer.mozilla​              |
| 0 == false                         | true   | false → 0. developer.mozilla+1​                                                      |
| 0 == ""                            | true   | "" → 0. stackoverflow+1​                                                             |
| 0 == "0"                           | true   | "0" → 0. developer.mozilla+1​                                                        |
| 0 == null                          | false  | null only equals undefined in ==. developer.mozilla+1​                               |
| 0 == undefined                     | false  | No special rule; stays unequal. developer.mozilla+1​                                 |
| null == false                      | false  | null does not coerce to boolean in ==. developer.mozilla+1​                          |
| undefined == false                 | false  | undefined only equals null for ==. developer.mozilla+1​                              |



Sure! Here’s a list of top JavaScript interview questions tailored for someone with 4+ years of experience, along with concise answers. These questions cover a range of topics including core concepts, advanced features, and best practices.

### Core Concepts

1. **What are closures in JavaScript?**
   - Closures are functions that have access to variables from another function’s scope. They enable functions to retain access to their lexical environment even when executed outside their scope.

2. **Explain the difference between `var`, `let`, and `const`.**
   - `var` has function scope and is hoisted. `let` and `const` have block scope and are not hoisted. `const` creates a read-only reference to a value.

3. **What is hoisting?**
   - Hoisting is JavaScript’s default behavior of moving declarations to the top of their scope before execution. This applies to `var` declarations and function declarations.

4. **What are promises?**
   - Promises are objects that represent the eventual completion or failure of an asynchronous operation and its resulting value. They provide a way to handle asynchronous operations in a more manageable way.

5. **What is async/await?**
   - `async` and `await` are syntactic sugar for handling promises. `async` functions return a promise, and `await` pauses the execution of the `async` function until the promise resolves.

6. **What is the event loop?**
   - The event loop is a mechanism that allows JavaScript to perform non-blocking operations by managing the execution of asynchronous tasks in a single-threaded environment.

7. **Explain the concept of prototypal inheritance.**
   - Prototypal inheritance allows objects to inherit properties and methods from other objects through their prototype chain.

8. **What is the difference between `==` and `===`?**
   - `==` performs type coercion before comparison, while `===` checks for both value and type equality without coercion.

9. **How does the `this` keyword work?**
   - `this` refers to the context in which a function is executed. Its value is determined by how a function is called, not where it is defined.

10. **What is the difference between `null` and `undefined`?**
    - `null` is an intentional absence of any object value, while `undefined` means a variable has been declared but not yet assigned a value.

### Advanced Topics

11. **What are JavaScript modules?**
    - Modules are a way to encapsulate code into reusable pieces, and they can be imported and exported between files. ES6 introduced `import` and `export` for module management.

12. **Explain the concept of "this" binding in different contexts.**
    - In global context, `this` refers to the global object. In a method, `this` refers to the object the method is called on. With `bind()`, `call()`, or `apply()`, `this` can be explicitly set.

13. **What is the "use strict" directive?**
    - `"use strict"` is a pragma that enables strict mode, which helps catch common coding mistakes and unsafe actions such as defining global variables accidentally.

14. **How does JavaScript handle asynchronous operations?**
    - JavaScript handles asynchronous operations using callbacks, promises, and async/await. These methods help manage tasks that take time to complete, like network requests.

15. **What is the purpose of the `bind()` method?**
    - `bind()` creates a new function that, when called, has its `this` keyword set to a specific value, with a given sequence of arguments preceding any provided when the new function is called.

16. **How do you debounce and throttle in JavaScript?**
    - Debouncing ensures that a function is not called too frequently by waiting until a pause in events. Throttling ensures a function is executed at most once in a specified time interval.

17. **What is a JavaScript generator function?**
    - A generator function, defined with `function*`, can be paused and resumed, allowing you to iterate over values with the `yield` keyword.

18. **Explain the difference between shallow copy and deep copy.**
    - A shallow copy duplicates only the top-level properties, while a deep copy duplicates every level of the object, including nested objects.

19. **What are the new data types introduced in ES6?**
    - ES6 introduced `Symbol`, which is a new primitive data type used to create unique and immutable identifiers.

20. **What are higher-order functions?**
    - Higher-order functions are functions that take other functions as arguments or return functions as results.

### Performance & Optimization

21. **How can you optimize the performance of JavaScript code?**
    - Techniques include minimizing DOM manipulations, using efficient algorithms, reducing network requests, and leveraging caching.

22. **What is the purpose of Web Workers?**
    - Web Workers allow JavaScript to run scripts in background threads, preventing blocking of the main thread and improving performance for intensive computations.

23. **Explain the concept of lazy loading.**
    - Lazy loading is a design pattern where resources are loaded only when they are needed, reducing initial load time and improving performance.

24. **What are memory leaks and how can you avoid them?**
    - Memory leaks occur when memory is not properly released. Common causes include global variables, closures that hold onto large data, and forgotten timers. Use tools like Chrome DevTools to identify and fix leaks.

25. **What are some common techniques for optimizing JavaScript code?**
    - Techniques include code minification, bundling, asynchronous loading of scripts, and avoiding excessive use of global variables.

### Frameworks & Libraries

26. **How does React’s Virtual DOM work?**
    - React’s Virtual DOM is a lightweight representation of the actual DOM. It enables efficient updates by diffing the Virtual DOM against the real DOM and applying only the necessary changes.

27. **What are the main differences between React and Angular?**
    - React is a library focused on building UI components and managing state, while Angular is a full-fledged framework with a comprehensive set of tools for building SPAs (Single Page Applications).

28. **What is a component lifecycle in React?**
    - The component lifecycle in React includes phases such as mounting, updating, and unmounting, with hooks or lifecycle methods available to execute code during these phases.

29. **Explain the concept of dependency injection in Angular.**
    - Dependency injection is a design pattern used to implement IoC (Inversion of Control). Angular uses it to provide services and other dependencies to components and other services.

30. **What is the purpose of `useEffect` in React?**
    - `useEffect` is a hook that allows you to perform side effects in function components, such as fetching data or subscribing to events.

### Practical & Best Practices

31. **How do you handle errors in JavaScript?**
    - Error handling can be managed using `try...catch` blocks, handling promise rejections with `.catch()`, and using global error handlers like `window.onerror`.

32. **What is the importance of code modularization?**
    - Modularizing code improves maintainability, readability, and reusability by dividing code into smaller, manageable, and logically grouped modules.

33. **How can you ensure code quality and consistency?**
    - Utilize linters (e.g., ESLint), formatters (e.g., Prettier), and adhere to coding standards and best practices.

34. **What are JavaScript design patterns, and can you name a few?**
    - Design patterns are reusable solutions to common problems. Examples include Singleton, Module, Observer, and Factory patterns.

35. **How do you manage state in a React application?**
    - State can be managed using local component state, Context API, or state management libraries like Redux or MobX.

36. **What is the purpose of the `event.preventDefault()` method?**
    - `event.preventDefault()` prevents the default action associated with an event from being executed, such as stopping a form submission or link navigation.

37. **What are some common security concerns in JavaScript and how do you address them?**
    - Common concerns include XSS (Cross-Site Scripting) and CSRF (Cross-Site Request Forgery). Use input validation, escaping, and proper authentication/authorization practices to mitigate risks.

38. **What is CORS and how does it work?**
    - CORS (Cross-Origin Resource Sharing) is a security feature implemented in browsers to allow or restrict resources requested from different origins. It works by sending specific HTTP headers to control cross-origin access.

39. **Explain the use of the `spread` and `rest` operators.**
    - The `spread` operator (`...`) expands an array or object into individual elements. The `rest` operator (`...`) collects multiple elements into an array or object.

40. **How can you test JavaScript code?**
    - JavaScript code can be tested using frameworks and tools like Jest, Mocha, Chai, and tools like Cypress for end-to-end testing.

### Advanced Techniques

41. **What is the `Proxy` object in JavaScript?**
    - The `Proxy` object allows you to create a proxy for another object, enabling custom behavior for fundamental operations like property access and method invocation.

42. **Explain the concept of "Event Delegation".**
    - Event delegation involves attaching a single event listener to a parent element to manage events for multiple child elements, improving performance and memory usage.

43. **What is the `Reflect` API in JavaScript?**
    - The `Reflect` API provides methods for interacting with objects and performing meta-programming tasks, such as intercepting and defining operations on objects.

44. **What is the

 difference between `Object.assign()` and the spread operator?**
    - `Object.assign()` copies properties from one or more source objects to a target object, while the spread operator creates a shallow copy of the object or array with additional properties.

45. **How do you implement a custom iterator in JavaScript?**
    - Implement a custom iterator by defining a `Symbol.iterator` method on an object, which returns an iterator object with a `next()` method.

46. **What are Symbols and how are they used?**
    - Symbols are unique and immutable primitive data types used primarily for creating unique property keys.

47. **What is a Service Worker and how is it used?**
    - A Service Worker is a script that runs in the background, separate from a web page, enabling features like offline access and background sync.

48. **How does JavaScript handle multi-threading?**
    - JavaScript handles multi-threading through Web Workers, which allow scripts to run in the background without blocking the main thread.

49. **What is a memory leak in JavaScript, and how can you prevent it?**
    - Memory leaks occur when memory is not properly released. Prevent leaks by managing scope, avoiding global variables, and properly cleaning up resources.

50. **Explain the concept of “template literals” and how they differ from regular strings.**
    - Template literals are string literals enclosed in backticks (\`\`), allowing for embedded expressions and multi-line strings using `${expression}` syntax.

