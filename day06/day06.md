# day06

## inferring within conditional types

`infer` 操作符
是一个 `helper`, 声明一个内部使用的泛型变量 (~~类型空间的私有变量?~~)

```typescript
type Flatten<Type> = Type extends Array<infer Item> ? Item : Type

// 也许你需要将下面这个写法，改写为不换行的三元运算符，会更容易理解
type GetReturnType<Type> = Type extends (...args: never[]) => infer Return
  ? Return
  : never
```

## distributive conditional types

[分配条件类型](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html#distributive-conditional-types)

```typescript
// 默认的行为是对 union 的每一个类型都进行同样的类型操作
type ToArray<Type> = Type extends any ? Type[] : never
type StrArrOrNumArr = ToArray<string | number> // string[] | number[]

// 下面的写法将 Type 作为一个整体，只进行一次类型操作
// 用理解 tuple 的行为会更好理解这种行为
type ToArrayNonDist<Type> = [Type] extends [any] ? Type[] : never
type StrArrOrNumArr = ToArrayNonDist<string | number> // (string | number)[]
```

## mapped types

映射类型 (批量操作)

```typescript
type OptionsFlags<Type> = {
  [Key in keyof Type]: boolean
}

type FeatureFlags = {
  darkMode: () => void
  newUserProfile: () => void
}

type FeatureOptions = OptionsFlags<FeatureFlags>
// type FeatureOptions = {
//   darkMode: boolean
//   newUserProfile: boolean
// }
```

### mapping modifiers

映射修饰符，默认是 `+`，意义为添加，可选 `-`，意义为去除。
出现在 `readonly`、`?` 之前

```typescript
type CreateMutable<Type> = {
  -readonly [Property in keyof Type]: Type[Property]
}

type LockedAccount = {
  readonly id: string
  readonly name: string
}

// readonly 被去除了
type UnlockedAccount = CreateMutable<LockedAccount>
```

### key remapping via as

改造 `Type` 的键名

```typescript
type MappedTypeWithNewProperties<Type> = {
  [Properties in keyof Type as NewKeyType]: Type[Properties]
}

// & 限定了 Property 必须是 string
type Getters<Type> = {
  [Property in keyof Type as `get${Capitalize<
    string & Property
  >}`]: () => Type[Property]
}
interface Person {
  name: string
  age: number
  location: string
}
type LazyPerson = Getters<Person>

// Remove the 'kind' property
type RemoveKindField<Type> = {
  [Property in keyof Type as Exclude<Property, "kind">]: Type[Property]
}

interface Circle {
  kind: "circle"
  radius: number
}

type KindlessCircle = RemoveKindField<Circle>

type EventConfig<Events extends { kind: string }> = {
  [E in Events as E["kind"]]: (event: E) => void
}

type SquareEvent = { kind: "square"; x: number; y: number }
type CircleEvent = { kind: "circle"; radius: number }

type Config = EventConfig<SquareEvent | CircleEvent>
```

### further rxploration

```typescript
type ExtractPII<Type> = {
  [Property in keyof Type]: Type[Property] extends { pii: true } ? true : false
}

type DBFields = {
  id: { format: "incrementing" }
  name: { type: string; pii: true }
}

type ObjectsNeedingGDPRDeletion = ExtractPII<DBFields>
```

## template literal types

模板字面量类型
只接受基本类型
拼接结果是笛卡尔积

```typescript
type World = "world"
type Greeting = `hello ${World}`

// 限制输入
type PropEventSource<Type> = {
  on(
    eventName: `${string & keyof Type}Changed`,
    callback: (newValue: any) => void,
  ): void
}

declare function makeWatchedObject<Type>(
  obj: Type,
): Type & PropEventSource<Type>

const person = makeWatchedObject({
  firstName: "Saoirse",
  lastName: "Ronan",
  age: 26,
})

person.on("firstNameChanged", () => {})

// Prevent easy human error (using the key instead of the event name)
person.on("firstName", () => {}) // error
```

### intrinsic string manipulation types

`typescript` 内置的操作 `string` 类型的类型工具

```typescript
// 大写
Uppercase<StringType>

// 小写
Lowercase<StringType>

// 首字母大写
Capitalize<StringType>

// 首字母小写
Uncapitalize<StringType>
```
