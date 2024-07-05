---
title: 'typescript总结笔记（3）装饰器'
date: 2023-03-09 21:42:13
tags: 
  - typescript
categories:
  - 程序员的自我修养
---
  <meta name="referrer" content="no-referrer">

## 一、装饰器

### 介绍

装饰器是一种特殊的语法，它允许你在不修改原始代码的情况下，向类、方法、属性或参数添加额外的功能或元数据。

装饰器通常以 `@` 符号开头，是一个方法。然后应用于目标（类、方法、属性或参数）。

### 装饰器方法的参数

+ **在类装饰器中**：

   - `target`：表示被装饰的类本身，通常是类的构造函数。你可以在类装饰器内部访问和操作整个类的构造函数和原型。

```js
function classDecorator(target: any) {
  // 在类装饰器中可以访问和操作类的构造函数和原型
}
```

+ **在方法装饰器中**：

   - `target`：表示被装饰方法所属的类的原型对象。你可以在方法装饰器内部访问和操作类的原型以及方法名。
   - `methodName`：表示被装饰的方法的名称。
   - `descriptor`：表示被装饰方法的属性描述符，包括方法的配置。

```js
function methodDecorator(target: any, methodName: string, descriptor: PropertyDescriptor) {
  // 在方法装饰器中可以访问和操作方法的原型、方法名和属性描述符
}
```

+ **在属性装饰器中**：

     - `target`：表示被装饰属性所属的类的原型对象。你可以在属性装饰器内部访问和操作类的原型以及属性名。
     - `propertyName`：表示被装饰的属性的名称。
     - `descriptor`：表示被装饰属性的属性描述符，包括属性的配置。
     
  ```js
  function propertyDecorator(target: any, propertyName: string, descriptor: PropertyDescriptor) {
    // 在属性装饰器中可以访问和操作属性的原型、属性名和属性描述符
  }
  ```
  
   
  
+ **在参数装饰其中**：

   - `target`：表示被装饰参数所属的类的原型对象。你可以在参数装饰器内部访问和操作类的原型以及参数所属的方法名。
   - `methodName`：表示包含参数的方法的名称。

```js
function parameterDecorator(target: any, methodName: string, parameterIndex: number) {
  // 在参数装饰器中可以访问和操作参数所属的方法的原型、方法名以及参数索引
}
```

### 类装饰器

```js
// 类装饰器示例
function classDecorator(target: any) {
  // 在类构造函数上添加元数据
  target.isDecorated = true;
}

// 应用类装饰器到类声明
@classDecorator
class Example {
  // 类定义
}

// 检查是否应用了类装饰器
console.log(Example.isDecorated); // 输出 true
```



### 方法装饰器

这里是一个**日志记录方法装饰器**的示例，作用为在方法调用前后记录日志。

```js
// 方法装饰器示例
function logMethod(target: any, methodName: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;

  // 修改方法，记录方法调用前后的日志
  descriptor.value = function (...args: any[]) {
    console.log(`Calling ${methodName} with arguments: ${JSON.stringify(args)}`);
    const result = originalMethod.apply(this, args);
    console.log(`${methodName} returned: ${result}`);
    return result;
  };
}

class Example {
  @logMethod
  add(a: number, b: number) {
    return a + b;
  }
}

const instance = new Example();
instance.add(2, 3);
// 输出：
// Calling add with arguments: [2,3]
// add returned: 5
```



### 属性装饰器

这里是一个**属性验证属性装饰器**的示例，验证属性的值是否为数字。

```js
// 属性装饰器示例
function validateNumber(target: any, propertyName: string) {
  let value: any;

  // 修改属性的 getter 和 setter，进行数字验证
  Object.defineProperty(target, propertyName, {
    get() {
      return value;
    },
    set(newValue) {
      if (typeof newValue !== 'number') {
        throw new Error(`Invalid value ${newValue} for property ${propertyName}. Must be a number.`);
      }
      value = newValue;
    },
  });
}

class Example {
  @validateNumber
  age: number;

  constructor(age: number) {
    this.age = age;
  }
}

const instance = new Example(25);
console.log(instance.age); // 输出 25
instance.age = 'thirty'; // 会抛出错误
```



### 参数装饰器

这里是一个**参数日志参数装饰器**的例子，用来记录方法参数的值。

```js
// 参数装饰器示例
function logParameter(target: any, methodName: string, parameterIndex: number) {
  const originalMethod = target[methodName];

  // 修改方法，记录方法参数的值
  target[methodName] = function (...args: any[]) {
    console.log(`Calling ${methodName} with arguments: ${JSON.stringify(args)}`);
    return originalMethod.apply(this, args);
  };
}

class Example {
  greet(@logParameter name: string) {
    console.log(`Hello, ${name}!`);
  }
}

const instance = new Example();
instance.greet('Alice');
// 输出：
// Calling greet with arguments: ["Alice"]
// Hello, Alice!
```



，
