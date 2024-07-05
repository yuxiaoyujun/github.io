---
title: 'typescript总结笔记（6）声明文件与声明语句'
date: 2023-03-10 22:21:13
tags: 
  - typescript
categories:
  - 程序员的自我修养
---
  <meta name="referrer" content="no-referrer">

## 一、声明文件与声明语句

### 1. 声明语句 `declare`

- declare 关键字用来告诉编译器，某个类型是存在的，可以在当前文件中使用。
- declare 只通知编译器某个类型是存在的，不用给出具体实现。
- declare 只能用来描述已经存在的变量和数据结构，不能用来声明新的变量和数据结构。

+ 所有 declare 语句都不会出现在编译后的文件里面。

### 场景

declare 关键字可以给出外部变量的类型描述，主要场景为下面两个：

+ **用于声明第三方库或模块的类型**：比如说引入第三方npm包时，若他们没有提供`.d.ts`文件。以`jQuery`为例，typescript将不知道如何处理jQuery对象，这时可以声明

  ```typescript
  // jQuery.d.ts
  declare let jQuery: (selector: string) => any
  ```

  现在，当你在项目中使用 jQuery 时，TypeScript 将知道它的类型信息，以便进行类型检查。

+ **与非 TypeScript 代码集成**：有些`js`库不包含`ts`代码，也没有声明文件（`*.d.ts`），需要自己书写声明文件（`*.d.ts`），此时在声明文件中声明全局变量，让`typescript`知道这些变量的类型。

### 可使用declare声明的类型

+ **声明外部变量** `declare var` 和 `declare let`

  ```typescript
  // *.d.ts
  declare let x: number
  declare var document
  
  declare let y:number = 1 // 报错，declare 关键字只用来给出类型描述
  ```
  使用方法：
  ```typescript
  // src/*.ts
  document.title = '标题'
  ```
  
+ **声明function **`declare function`

  ```
  declare function fn (str:string): void
  ```

+ **声明class** `declare class`

  ```typescript
  declare class Animal {
  	constructor(): void
    public static setInit(): string
  	public sayHello(): void
    public getName(name: string): string
  }
  ```
  
+ **声明模块和命名空间** `declare namespace` 和 `declare module`

  ```typescript
  declare namespace AnimalLib {
    class Animal {
      constructor(name:string);
      eat():void;
      sleep():void;
    }
  
    type Animals = 'Fish' | 'Dog';
  }
  ```

  ```typescript
  declare module AnimalLib {
    class Animal {
      constructor(name:string);
      eat(): void;
      sleep(): void;
    }
  
    type Animals = 'Fish' | 'Dog';
  }
  ```

+ **声明js原生属性和方法 ** `declare global`

  为 JavaScript 引擎的原生对象添加属性和方法，可以使用`declare global {}`语法。
  
  ```typescript
  declare global {
  	interface String {
  		getLength(): number  // 接口中可以这样来声明方法
  	}
  }
  
  export {};  // 必须要写，在declare global时必须要告诉typescipt这是一个模块。否则会报错
  ```
  
  使用：
  
  ```typescript
  let a: string = "sdfsdf";
  a.getLeng();
  ```
  
  当然，必须要在`tsconfig`中的`include`或`files`中进行引入。
  
  ```typescript
  {
      "include": ["src/**/*.ts"],  // 数组，可以使用通配符
      "files": [
        "string.d.ts" // 数组，必须是具体文件
      ],
  	}
  ```
  声明文件比较多时，可以使用三斜线指令（下面会说）
  ···
  /// <reference path="../string.d.ts" />
  ···
+ **声明`enmu`** `declare enmu`

  ```typescript
  declare Color {
  	Red,
  	Blue,
  	Green
  }
  ```

  

### 2.  声明文件 `.d.ts`

### 2.1 概念

一般来说，声明语句会被放到一个单独的文件（`*.d.ts`）中，这就是声明文件。

### 2.2 第三方声明文件

第三方库一般提供了声明文件，如`jQuery`。安装第三方声明文件的方法是：

```bash
npm install @types/xxx --save-dev
```

如jQuery的声明文件

```bash
npm install @types/jquery --save-dev
```

一般来说`typescript`可以自动识别`@types/` 的声明文件并加载，但如果官方未提供声明文件，但社区有提供，此时想更改编译选项，可以在`tscofig`中配置：

```typescript
{
  "compilerOptions": {
    "typeRoots": ["./typings"], // 社区下载的非官方声明文件
    "types" : ["jquery"] // 引入node_modules/**/@types对应的官方文件，一般不比写
  }
}
```

+ 补充一点ts查找声明文件的顺序：
  - 首先会查找 typeRoots 中指定的目录，以查找自定义的声明文件。
  - 然后，会查找 types 中指定的声明文件，这通常是用于引入官方声明文件或项目内部的自定义声明文件。
  - 最后，会继续查找 node_modules/@types 目录下的官方声明文件。

### 2.3 项目自动生成声明文件

`tsconfig`中，配置`"declaration": true` ，编译 时会 自动生成 。

```typescript
{
  "compilerOptions": {
    "declaration": true
  }
}
```

或命令行

```bash
tsc --declaration
```

### 2.4 内置声明文件

在安装`typescript`的文件夹内（一般是项目的`node_modules/typescript`中），会有`lib`文件夹，提供了一些常见的声明文件。

![](/images/image-20231008034339747.png)

可以在`tsconfig`中启用

```typescript
{
  "compilerOptions": {
    "lib": ["dom"]
  }
}
```

### 2.5 自己书写声明文件

如果既没有官方声明文件，也没有第三方社区声明文件，那只好自己写了。

declare在上面👆🏻





