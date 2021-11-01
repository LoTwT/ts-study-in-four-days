# day01

[typescript 官网](https://www.typescriptlang.org/)

## introduction

typescript 面向类型编程，操作都在类型空间内完成。~~换了个地方内卷 😝~~

## install

```shell
pnpm add typescript -g
```

## tsc

```shell
# 生成 tsconfig.json
tsc --init
# 生成中文注释的 tsconfig.json
tsc --init --locale zh-cn
```

## duck typing

> “当看到一只鸟走起来像鸭子、游泳起来像鸭子、叫起来也像鸭子，那么这只鸟就可以被称为鸭子。”

[鸭子类型](https://baike.baidu.com/item/%E9%B8%AD%E5%AD%90%E7%B1%BB%E5%9E%8B/10845665?fr=aladdin)

```typescript
type Point = {
  x: number
  y: number
}

function foo(p: Point) {
  console.log(p.x, p.y)
}

foo({ x: 1, y: 2 })
```

## type alias

类型别名:

```typescript
type Person = {
  name: string
  age: number
}

type Student = Person & { score: number }
```

## interface

接口:

```typescript
interface Person {
  name: string
  age: number
}

interface Student extends Person {
  score: number
}
```

## rules

- 可选类型: `{ x?: number }` 等同于 `{ x: number | undefined }`
- 联合类型: `type Color = "red" | "blue"`
- 交叉类型: `type Color = "red" & "blue"`
- 类型收缩 (narrowing)

  ```typescript
  const print = (id: number | string) => {
    if (typeof id === "string") {
      console.log(id.toUpperCase())
    } else if (typeof id === "number") {
      console.log(id.toFixed(2))
    }
  }
  ```

## demo

关键字会影响 typescript 的类型推断，使其更符合语义

```typescript
const message1 = "message1" // 此处 message1 类型为字面量 "message1"
let message2 = "message2" // 此处 message2 类型为 string
```
