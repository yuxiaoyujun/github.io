---
title: 'typescript总结笔记（4）类型工具、类型映射、类型运算符'
date: 2023-03-09 21:42:13
tags: 
  - typescript
categories:
  - 程序员的自我修养
---
  <meta name="referrer" content="no-referrer">

## 一、类型工具

TypeScript 提供了一些内置的类型工具，用来方便地处理各种类型，以及生成新的类型。

`Exclude<T, U>` 从 `T` 中排除出可分配给 `U`的元素。
 `Omit<T, K>` 的作用是忽略`T`中的某些属性。
 `Merge<O1, O2>` 是将两个对象的属性合并。
 `Compute<A & B>` 是将交叉类型合并
 `Intersection<T, U>`的作用是取`T`的属性,此属性同样也存在与`U`。
 `Overwrite<T, U>` 是用`U`的属性覆盖`T`的相同属性。

## `Pick<Type, Keys>`

`Pick<Type, Keys>`返回一个新的对象类型，第一个参数`Type`是一个对象类型，第二个参数`Keys`是`Type`里面被选定的键名。

```js
interface A {
  x: number;
  y: number;
}

type T1 = Pick<A, 'x'>; // { x: number }
type T2 = Pick<A, 'y'>; // { y: number }
type T3 = Pick<A, 'x'|'y'>;  // { x: number; y: number }
```

上面示例中，`Pick<Type, Keys>`会从对象类型`A`里面挑出指定的键名，组成一个新的对象类型。

指定的键名`Keys`必须是对象键名`Type`里面已经存在的键名，否则会报错。

```js
interface A {
  x: number;
  y: number;
}

type T = Pick<A, 'z'>; // 报错
```

上面示例中，对象类型`A`不存在键名`z`，所以报错了。

`Pick<Type, Keys>`的实现如下。

```js
type Pick<T, K extends keyof T> = {
  [P in K]: T[P];
};
```



## `Omit<Type, Keys>`

`Omit<Type, Keys>`用来从对象类型`Type`中，删除指定的属性`Keys`，组成一个新的对象类型返回。

```js
interface A {
  x: number;
  y: number;
}

type T1 = Omit<A, 'x'>;       // { y: number }
type T2 = Omit<A, 'y'>;       // { x: number }
type T3 = Omit<A, 'x' | 'y'>; // { }
```

上面示例中，`Omit<Type, Keys>`从对象类型`A`里面删除指定属性，返回剩下的属性。

指定删除的键名`Keys`可以是对象类型`Type`中不存在的属性，但必须兼容`string|number|symbol`。

```js
interface A {
  x: number;
  y: number;
}

type T = Omit<A, 'z'>; // { x: number; y: number }
```

上面示例中，对象类型`A`中不存在属性`z`，所以就原样返回了。

`Omit<Type, Keys>`的实现如下。

```js
type Omit<T, K extends keyof any> 
  = Pick<T, Exclude<keyof T, K>>;
```

## `Exclude<UnionType, ExcludedMembers>`

`Exclude<UnionType, ExcludedMembers>`用来从联合类型`UnionType`里面，删除某些类型`ExcludedMembers`，组成一个新的类型返回。

```typescript
type T1 = Exclude<'a'|'b'|'c', 'a'>; // 'b'|'c'
type T2 = Exclude<'a'|'b'|'c', 'a'|'b'>; // 'c'
type T3 = Exclude<string|(() => void), Function>; // string
type T4 = Exclude<string | string[], any[]>; // string
type T5 = Exclude<(() => void) | null, Function>; // null
type T6 = Exclude<200 | 400, 200 | 201>; // 400
type T7 = Exclude<number, boolean>; // number
```

`Exclude<UnionType, ExcludedMembers>`的实现如下。

```typescript
type Exclude<T, U> = T extends U ? never : T;
```

上面代码中，等号右边的部分，表示先判断`T`是否兼容`U`，如果是的就返回`never`类型，否则返回当前类型`T`。由于`never`类型是任何其他类型的子类型，它跟其他类型组成联合类型时，可以直接将`never`类型从联合类型中“消掉”，因此`Exclude<T, U>`就相当于删除兼容的类型，剩下不兼容的类型。

## `Extract<Type, Union>`

`Extract<UnionType, Union>`用来从联合类型`UnionType`之中，提取指定类型`Union`，组成一个新类型返回。它与`Exclude<T, U>`正好相反。

```typescript
type T1 = Extract<'a'|'b'|'c', 'a'>; // 'a'
type T2 = Extract<'a'|'b'|'c', 'a'|'b'>; // 'a'|'b'
type T3 = Extract<'a'|'b'|'c', 'a'|'d'>; // 'a'
type T4 = Extract<string | string[], any[]>; // string[]
type T5 = Extract<(() => void) | null, Function>; // () => void
type T6 = Extract<200 | 400, 200 | 201>; // 200
```

如果参数类型`Union`不包含在联合类型`UnionType`之中，则返回`never`类型。

```typescript
type T = Extract<string|number, boolean>; // never
```

`Extract<UnionType, Union>`的实现如下。

```typescript
type Extract<T, U> = T extends U ? T : never;
```

## `Partial<Type>`

`Partial<Type>`返回一个新类型，将参数类型`Type`的所有属性变为可选属性。

```typescript
interface A {
  x: number;
  y: number;
}
 
type T = Partial<A>; // { x?: number; y?: number; }
```

`Partial<Type>`的实现如下。

```typescript
type Partial<T> = {
  [P in keyof T]?: T[P];
};
```

## `Readonly<Type>`

`Readonly<Type>`返回一个新类型，将参数类型`Type`的所有属性变为只读属性。

```ts
interface Todo {
  title: string;
}

const todo: Readonly<Todo> = {
  title: "Delete inactive users",
};

todo.title = "Hello";
// TypeError: Cannot assign to 'title' because it is a read-only property.
```
## `Required<Type>` 

`Required<Type>`返回一个新类型，将参数类型`Type`的所有属性变为必选属性。它与`Partial<Type>`的作用正好相反。

```typescript
interface A {
  x?: number;
  y: number;
}

type T = Required<A>; // { x: number; y: number; }
```

`Required<Type>`的实现如下。

```typescript
type Required<T> = {
  [P in keyof T]-?: T[P];
};
```

上面代码中，符号`-?`表示去除可选属性的“问号”，使其变成必选属性。

相对应地，符号`+?`表示增加可选属性的“问号”，等同于`?`。因此，前面的`Partial<Type>`的定义也可以写成下面这样。

```typescript
type Partial<T> = {
  [P in keyof T]+?: T[P];
};
```

## `Record<Keys, Type>` 

构造一个类型，它的所有key是Keys类型，所有value是Type类型
```ts
interface Employee {
  name: string;
  age: number;
}
let employee1: Record<number, Employee> = {
  0: { name: "zhao", age: 11111 },
  1: { name: "qian", age: 22222 },
  2: { name: "sun", age: 33333 },
  3: { name: "li", age: 44444 },
};
type Key = "zhaoID" | "qianID" | "sunID" | "liID";
let employee2: Record<Key, Employee> = {
  zhaoID: { name: "zhao", age: 11111 },
  qianID: { name: "qian", age: 22222 },
  sunID: { name: "sun", age: 33333 },
  liID: { name: "li", age: 44444 },
};
```
