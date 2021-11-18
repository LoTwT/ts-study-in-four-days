# day05

## Generics

泛型

```typescript
function returnArg<T>(arg: T): T {
  return arg
}

let num = returnArg(1) // number
let str = returnArg("helloworld") // string

// 字面量类型也可以作为泛型
const num = returnArg(1) // 1
const str = returnArg("helloworld") // "helloworld"
```

### generic constraints

使用 `extends` 限制泛型的类型

```typescript
function printLength<T extends { length: number }>(obj: T): number {
  return obj.length
}
```

### generic function

泛型函数

```typescript
// 普通函数
function foo<T>(p: T): T {
  return p
}

// 箭头函数
const bar = <T>(p: T): T => p
```

### generic interface

泛型接口

```typescript
// 接口声明泛型函数类型
interface FooFn {
  <T>(p: T): T
}

interface BarFn<T> {
  (p: T): T
}
```

### generic class

泛型类

```typescript
class BoxContainer<T> {
  item: T
  print: (item: T) => void
}
```

### using type parameters in generic constraints

在泛型约束中使用类型参数

```typescript
function getProperty<T, K extends keyof T>(obj: T, prop: K) {
  return obj[prop]
}

const person = {
  name: "lo",
  age: 24,
}

getProperty(person, "name")
getProperty(person, "age")
```

### using class types in generics

在泛型中使用类类型

```typescript
// interface
function create<T>(construct: { new (): T }) {
  return new construct()
}

// function
function create<T>(construct: new () => T) {
  return new construct()
}
```

## keyof type operator

`keyof` 操作符

```typescript
type Person = {
  name: string
  age: number
}
type Keys = keyof Person // Keys: "name" | "age"
```

js 中对象的 key 会被强制转换为字符串，所以 `arr[0]` 和 `arr["0"]` 相同。(说的就是你，**数组**)

## typeof type operator

类型空间的 `typeof`
更推荐对**变量**使用
如果需要函数返回值类型，请使用 `ReturnType<>`

```typescript
const person = {
  name: "lo",
  age: 24,
}
type Person = typeof person
```

## indexed access types

```typescript
type Person = {
  name: string
  age: number
}
type Age = Person["age"]

const students = [
  { name: "xiaoming", age: 10 },
  { name: "xiaohong", age: 11 },
  { name: "xiaogang", age: 12 },
]
type Student = typeof students[number]
type StudentName = typeof students[number]["name"]
type StudentAge = typeof students[number]["age"]
type StudentGender = typeof students[number]["gender"]
```

## condition types

条件类型 (绝大多数情况下能够代替**函数重载**)

```typescript
interface Animal {
  live(): void
}

interface Dog extends Animal {
  woof(): void
}

type Example1 = Dog extends Animal ? number : string

type NameOrId<T extends number | string> = T extends number
  ? IdLabel
  : NameLabel

type MessageOf<T> = T extends { message: unknown } ? T["message"] : never

type Flatten<T> = T extends any[] ? T[number] : T
```
