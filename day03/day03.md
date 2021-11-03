# day03

## Function Type Expressions

```typescript
type FooFn = (p: string) => void
interface BarFn {
  (p: number): void
}
```

## Call Signatures

```typescript
type DescribableFunction = {
  description: string
  (someArg: number): boolean
}

function doSomething(fn: DescribableFunction) {
  console.log(fn.description + " returned " + fn(6))
}
```

## Construct Signatures

```typescript
interface PersonConstructor {
  new (name: string, age: number): void
}

class Person {
  constructor(public name: string, public age: number) {}
}

const a: PersonConstructor = Person
```

## Generic Functions

```typescript
function create<T>(construct: new () => T) {
  return new construct()
}

const numebrArr = create<number[]>(Array)
```

## Generic Constraints

```typescript
function getLength<T extends { length: number }>(p: T) {
  return p.length
}
```

## Guidelines for Writing Good Generic Functions

- 绝大多数时候都可以用 `union` 代替 `Generic`
- 更少使用泛型参数
- 更准确定义 `Generic` 的样子

## Optional Parameters

```typescript
// x?: number 等同于 x: number | undefined
function f1(x?: number) {}
```

### Optional Parameters in Callbacks

- 回调函数需要完整声明所有会用到的实参
- 回调函数调用时，实参数量**小于等于**形参数量 (~~参数声明了可以不使用~~)

## Function Overloads

函数重载 (~~大多数的函数重载都可以使用 `union` 替代~~)

```typescript
type Nullable<T> = T | null
function add(a: number, b: number): number
function add(a: string, b: string): string
function add(
  a: number | string,
  b: number | string,
): Nullable<number | string> {
  if (typeof a === "number" && typeof b === "number") {
    return a + b
  } else if (typeof a === "string" && typeof b === "string") {
    return a + b
  }
  return null
}
```

## unknown

- `unknown` 是未知值，任何未知值都是不合法的
- 简单地说是 `安全的 any`，提醒使用者需要注意这个值
- 使用类型断言去改变 `unknown`

```typescript
async function request(): Promise<unknown> {
  // todo
}

const data = await request()
data.a = 10 // 报错: 对象的类型为 "unknown"
```

## Function

`Function`: 一个抽象的函数类型，只具备 JS 中函数的基本属性和基本方法

- 随意使用 `Function` 是不够安全的
- 如果参数是个函数，但你不想调用它，使用 `() => void` 更安全

## tuple

```typescript
function useState<T>(data: T): [T, setFn<T>] {
  // todo
  return [state, setState]
}

// args 的类型是 [8, 5] 的 tuple
const args = [8, 5] as const
```

## Parameter Destructuring

```typescript
type People = {
  name: string
  age: number
  score: number
}

function printScore({ score }: People) {
  console.log(score)
}
```

## Assignability of Functions

```typescript
type noop = () => void
const foo: noop = () => true
```

即使使用 `() => void` 作为类型，函数返回值仍可以是任意类型；这是一种不正常，但是意料之中的行为。
个人认为 `() => void` 的值实现为 `function() {}`，这样能够统一箭头函数和普通函数的调用行为。
例如 `Array.prototype.push` 返回 `number`， 而 `Array.prototype.forEach` 的返回类型是 `void`
