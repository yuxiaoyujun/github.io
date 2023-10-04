---
title: 'typescript总结笔记(2)'
date: 2023-03-08 19:24:13
tags: 
  - typescript
categories:
  - 程序员的自我修养
---
  <meta name="referrer" content="no-referrer">

## 一、函数类型表达式
描述函数可以使用函数类型表达式。
格式：
```js
fn: (arg: string) => void
```
比如在接口中使用
```js
interface pringStrToConsole {
  str: string;
  printStr: (str: string) => void;
  // printStr: (string)=>void 不可以这么写，这么写表示函数有一个参数any，类型为string
}
```
```js
let ps: pringStrToConsole = {
  str: "aaa",
  printStr: (str) => {
    console.log(str);
  },
};
ps.printStr(ps.str);
```
或使用类型别名（type）定义一个函数类型
```js
type GreetFunction = (string)=>void
```
### 调用签名
由于在js中，函数不仅可以被调用，函数本身也可以具有属性值。用**函数类型表达式**无法支持声明属性，这时候可以使用**调用签名**
格式：
```js
(str: string): void
```
具体应用：
```js
type DescribableFunction = {
  description: string,
  (str: string): void
}
function doSomething(fn: DescribableFunction) {
  console.log(fn.description + " returned " + fn(6));
}
```
### 构造签名
即，使用new Function时的用法，构造函数。
说实话，我没看太懂
```js
type SomeConstructor = {
  new (s: string): SomeObject;
};
function fn(ctor: SomeConstructor) {
  return new ctor("hello");
}
```

### 泛型函数
定义一个函数，返回数组的第一个元素。
由于数组的类型不一定，用any没有用泛型好。
泛型和强制类型转换都是尖括号`<>`，泛型放在变量后，强制类型转换放在变量前。
```js
function firstElement<Type>(arr: Type[]): Type | undefined {
  return arr[0];
}
```
```js
let strArr: string[] = ["aaa", "bbb", "ccc"];
let fe1 = firstElement(strArr); // aaa

let nArr: number[] = [111, 222, 333];
let fe2 = firstElement(nArr); // 111
```

## 二、对象类型
## 三、泛型
```js
function consoleLog<T,Y>(content: T | Y): void {
  console.log(content)
}
```
## 四、工具类型
### 1. Partial
构造一个类型，所有的属性均为可选。
```ts
interface TODO {
  title: string;
  description: string;
}

type TODO2 = Partial<TODO>
let todo2: TODO2 = {
  title: 'sdfsdfsdf'
}
```
### 2. Required
构造一个类型，所有的属性均为必选
```ts
interface Props {
  a?: number;
  b?: string;
}
const obj: Props = { a: 5 };
const obj2: Required<Props> = { a: 5 };
```
### 3. Readonly
构造一个类型，所有的属性均为只读
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
### 4. Record<Keys, Type>
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
```
```ts
type Key = "zhaoID" | "qianID" | "sunID" | "liID";
let employee2: Record<Key, Employee> = {
  zhaoID: { name: "zhao", age: 11111 },
  qianID: { name: "qian", age: 22222 },
  sunID: { name: "sun", age: 33333 },
  liID: { name: "li", age: 44444 },
};
```
### 5. Pick<Type, Keys>
构造一个类型，保留Type中的Keys属性。
### 6. Omit<Type, Keys>
构造一个类型，从Type中过滤掉Keys属性
### 7. Exclude<UnionType, ExcludedMembers>
构造一个类型，从UnionType中排除所有可以赋给ExcludedMembers的类型。
其中UnionType和ExcludedMembers都是**联合类型**。
### 8. Extract<Type, Union>
构造一个类型，从Type中提取所有可以赋给Union的类型