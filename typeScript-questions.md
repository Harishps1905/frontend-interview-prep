# Top 50 TypeScript Interview Questions

TypeScript interview questions emphasize core types, advanced utilities, and React integration for senior frontend roles. These 50 questions include answers and code examples tailored for 6+ years experience, focusing on job prep. Practice verbal explanations alongside coding.

## Basic Types \& Declarations

- **What are TypeScript's basic types?**
Includes `boolean`, `number`, `string`, `array`, `tuple`, `enum`, `any`, `void`, `null`, `undefined`, `never`, `object`.

```ts
let age: number = 30;
let scores: number[] = [85, 90];
```

- **let vs const difference?**
`let` reassignable with block scope; `const` blocks reassignment but allows object mutation.

```ts
const arr = [1]; arr.push(2); // Valid
```

- **Type inference example?**
TS infers types automatically from assignments.

```ts
let value = 10; // Inferred: number
```


## Interfaces \& Types

- **Interface definition?**
Contracts object shapes for type safety.

```ts
interface Point { x: number; y: number; }
let p: Point = { x: 3, y: 7 };
```

- **Interface vs type alias?**
Interfaces for extendable objects; types for unions/primitives.

```ts
type ID = string | number;
interface User { id: ID; name: string; }
```

- **Extend interface?**
Use `extends` keyword.

```ts
interface Animal { name: string; }
interface Dog extends Animal { breed: string; }
```


## Functions \& Generics

- **Function typing?**
Parameter and return types specified.

```ts
function add(a: number, b: number): number {
  return a + b;
}
```

- **Generics purpose?**
Reusable type-safe components.

```ts
function identity<T>(arg: T): T { return arg; }
identity<string>("hello");
```

- **Rest parameters?**
Variable arguments via `...`.

```ts
function sum(...nums: number[]): number {
  return nums.reduce((a, b) => a + b, 0);
}
```


## Classes \& OOP

- **Access modifiers?**
`public` (default), `private` (class), `protected` (class + subclasses).

```ts
class Person {
  private age: number = 30;
}
```

- **Inheritance?**
`extends` and `super()`.

```ts
class Animal { move() {} }
class Snake extends Animal {
  move() { super.move(); }
}
```

- **Abstract classes?**
Partial implementations with abstract methods.

```ts
abstract class Shape {
  abstract getArea(): number;
}
```


## Advanced Types

- **Union types?**
Multiple possible types.

```ts
let id: string | number = "abc";
id = 123;
```

- **Intersection types?**
Merge multiple types.

```ts
type Admin = User & { role: 'admin'; };
```

- **Discriminated unions?**
Unions with literal discriminant.

```ts
type State = { status: 'loading' } | { status: 'success', data: any };
```


## Utility Types

| Utility | Purpose | Example |
| :-- | :-- | :-- |
| `Partial<T>` | Optional properties | `Partial<User>` |
| `Required<T>` | All required | `Required<User>` |
| `Readonly<T>` | Immutable | `Readonly<User>` |
| `Pick<T, K>` | Select keys | `Pick<User, 'name' \| 'age'>` |
| `Omit<T, K>` | Exclude keys | `Omit<User, 'age'>` |
| `Record<K, T>` | Key-value object | `Record<string, number>` |

## Optional Chaining \& Safety

- **Optional chaining (`?.`)?**
Safe property access.

```ts
const city = user?.address?.city ?? 'Unknown';
```

- **unknown vs any?**
`unknown` requires type guards; `any` skips checks.

```ts
let x: unknown;
if (typeof x === 'string') console.log(x.toUpperCase());
```


## Modules \& Namespaces

- **Export/Import?**
ES module syntax.

```ts
// types.ts
export interface User { name: string; }
// main.ts
import { User } from './types';
```

- **Namespaces?**
Code organization (legacy).

```ts
namespace Utils {
  export function log(msg: string) {}
}
Utils.log('hi');
```


## Error Handling

- **Never type?**
Unreachable code paths.

```ts
function fail(msg: string): never {
  throw new Error(msg);
}
```

- **Type guards?**
Narrow union types.

```ts
function isString(x: any): x is string {
  return typeof x === 'string';
}
```


## Senior-Level Advanced

- **Mapped types?**
Transform all properties.

```ts
type Readonly<T> = { readonly [P in keyof T]: T[P]; };
```

- **Conditional types?**
Type-level if/else.

```ts
type NonNullable<T> = T extends null | undefined ? never : T;
```

- **Type-safe API?**
Generic responses.

```ts
type ApiResponse<T> = { success: true; data: T } | { success: false; error: string };
```


## tsconfig.json Essentials

- **Key options?**
`"strict": true`, `"target": "ES2020"`, `"noImplicitAny": true`, `"outDir": "dist"`.
- **Compile?**
`npx tsc --watch` for development.


## React/Next.js Specific

- **Typed props?**

```ts
interface Props { name: string; age?: number; }
const Component: React.FC<Props> = ({ name }) => <div>{name}</div>;
```

- **useState typing?**

```ts
const [user, setUser] = useState<User | null>(null);
```

- **Generic custom hook?**

```ts
function useFetch<T>(url: string): { data: T | null; loading: boolean } {
  // Fetch logic
  return { data: null, loading: true };
}
```


## Additional Senior Questions (31-50)

- **Infer keyword?**
Extract types from arguments. `type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never;`
- **Template literal types?**
String manipulation. `type EventName = `on\${'Click' | 'Hover'};`→`'onClick' | 'onHover'`
- **Keyof operator?**
Union of keys. `type Keys = keyof User;`
- **In operator?**
Conditional on key existence. `K extends keyof T ? T[K] : never`
- **Exclude/Extract?**
`Exclude<string \| number, number>` → `string`


