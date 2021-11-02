# day02

## Type Assertions

类型断言

```typescript
const canvas = document.getElementById("canvas") as HTMLCanvasElement
const canvas = <HTMLCanvasElement>document.getElementById("canvas")
```

- 更推荐 `as`，防止与 `tsx` 冲突

## Literal Types

字面量类型

```typescript
const message = "message" // 类型是 "message" 字面量
let greet = "hello" // 类型是 string
```

设置单个字面量类型通常没有很大的意义，更适合在 `Type Alias` 中使用 `union`

```typescript
type Directions = "top" | "right" | "bottom" | "left"
```

字面量推断

```typescript
// 类型会被推断为 { url: string, method: string }
const req = { url: "http://httpbin.org/get", method: "GET" }
// 在以下调用中会出错
handleRequest(req) // string 不能赋给 "GET" | "POST"

// 解决办法
// 1
type REQ = {
  url: string
  method: "GET" | "POST"
}
const req: REQ = { url: "http://httpbin.org/get", method: "GET" }
const req = { url: "http://httpbin.org/get", method: "GET" } as REQ

// 2
const req = { url: "http://httpbin.org/get", method: "GET" as "GET" }
const req = { url: "http://httpbin.org/get", method: "GET" } as const
```

## null & undefined

**无论何时，都推荐打开 `strictNullChecks`**

```typescript
// 小工具
type Nullable<T> = T | null
```

## Non-null Assertion Operator

非空断言运算符 **(只能在 TS 中使用)**

```typescript
// !!! 以下方法只是在编译期不进行类型检查
// 在运行时仍有可能为 null，谨慎使用
function foo(id: Nullable<number>) {
  // no error
  console.log(x!.toFixed())
}
```

## enum

枚举 **(TS 独有)**

```typescript
enum Directions {
  TOP, // 0
  RIGHT, // 1
  BOTTOM = 5, // 5
  LEFT, // 6
}

enum StringDirections {
  TOP = "TOP",
  RIGHT = "RIGHT",
  BOTTOM = "BOTTOM",
  LEFT = "LEFT",
}
```

## narrowing

```typescript
// 1. typeof
function padLeft(padding: number | string, input: string): string {
  if (typeof padding === "number") {
    return new Array(padding + 1).join(" ") + input
  }
  return padding + input
}

// 2. truthy, falsy => 0 NaN null undefined 0 ""
// if (truthy) {}

// 3. ===、!==

// 4. in
type Fish = { swim: () => void }
type Bird = { fly: () => void }

function move(animal: Fish | Bird) {
  if ("swim" in animal) {
    return animal.swim()
  }
  return animal.fly()
}

// 5. instanceof
function logValue(x: Date | string) {
  if (x instanceof Date) {
    console.log(x.toUTCString())
  } else {
    console.log(x.toUpperCase())
  }
}
```

## Control flow analysis

控制流分析，基于可达性进行分析

## Using type predicates

```typescript
function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined
}

if (isFish(pet)) {
  pet.swim()
} else {
  pet.fly()
}
```

## Discriminated unions

```typescript
interface Circle {
  kind: "circle"
  radius: number
}

interface Square {
  kind: "square"
  sideLength: number
}

type Shape = Circle | Square

function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2
    case "square":
      return shape.sideLength ** 2
  }
}
```

## never

```typescript
function foo(id: string | number) {
  if (typeof id === "string") {
    id.toUpperCase()
  } else if (typeof id === "number") {
    id.toFixed()
  } else {
    // id => never
  }
}
```
