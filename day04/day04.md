# day04

## Optional Properties

可选属性

```typescript
interface PaintOptions {
  shape: Shape
  xPos?: number
  yPos?: number
}

// 值空间中，以下写法是修改属性名，和类型冲突，所以会报错！
function foo({ shape: Shape, xPos: number = 1 }) {} // error
```

## readonly Properties

```typescript
interface Student {
  readonly name: string
  readonly score: string
}
```

## Index Signatures

索引签名

代表一类属性的样子，`key` 和 `value` 不完全确定

由于 `javascript` 的特性，`arr[0]` 会被认为是 `arr["0"]`

```typescript
interface Fruit {
  [name: string]: string
}
```

## Extending Types

interface 继承支持多继承

```typescript
interface People {
  name: string
  age: number
}

interface Student extends People {
  score: number
  study: () => void
}
```

## Intersection Types

交叉类型

```typescript
type Color = {
  color: "red" | "blue"
}

type Circle = {
  shape: "Circle"
}

type ColorfulCircle = Color & Circle
```

## extends 和 & 的重复处理

```typescript
type User = {
  name: string
}

type People = User & {
  name: number
}

// 报错 name: never
const p: People = {
  name: "string",
}
```

```typescript
interface User {
  name: string
}

// 报错 类型不兼容
interface People extends User {
  name: number
}
```

## Generic Object Types

```typescript
interface Box<T> {
  contents: T
}
type Box1<T> = {
  contents: T
}

type StringBox = Box<string>
type NumberBox = Box1<number>

function setContents<T>(box: Box<T>, contents: T) {
  box.contents = contents
}
```

## ReadonlyArray

~~ReadonlyArray~~

```typescript
const roArray: ReadonlyArray<string> = ["red", "green", "blue"]
const roArray1: readonly string[] = ["red", "green", "blue"]
roArray.push("yellow") // error
```

## Tuple

元组

在**基于约定**的 API 中非常有用 => **React Hooks**

```typescript
const pair: [string, number] = ["123", 123]

interface TuplePair {
  length: 2
  0: string
  1: number
}

// optional
const group: [string, number, boolean?] = ["123", 123]

// readonly
const point: readonly [number, number] = [1, 2]
```
