---
title: 'typescript总结笔记（5）命名空间、模块'
date: 2023-03-09 21:42:13
tags: 
  - typescript
categories:
  - 程序员的自我修养
---
  <meta name="referrer" content="no-referrer">

## 一、命名空间和模块

### 1. 命名空间的意义

原先叫内部空间。

当多人开发同一文件时可能会造成重名或覆盖等问题。此时可以使用`namespace`在同一文件里进行不同开发。即使方法名称相同，但在不同的`namespace`中就可以避免互相覆盖。

+ `namespace`中定义的东西只能在该`namespace`中使用。若需要在外部使用则需要先`export`
+ 在别的`namespace`中使用该`namespace`的方法，需要`import`，`import`可以在`namespace`外使用。
+ `namespace`可以导出到其他文件使用，方法有两种：
  + 可以使用模块（原先叫外部空间）导出与导入（`export`与`import`），
  + 还有一种不是很推荐的方法，用三斜线指令导入。`/// <reference path = "SomeFileName.ts" />`
  + 其实命名空间导出到其他文件使用这种事情，我本身就不是很推荐。。。这种情况可以直接用模块了。

### 代码演示：

```typescript
namespace Utils {
	function isString(value: unknown) {
		return typeof value === "string";
	}
	export function isStringForExport(value: unknown) {
		return typeof value === "string";
	}
	isString("aaa"); // true
}

// `import`可以在`namespace`外使用。
import isStr = Utils.isStringForExport;

// Utils.isString('aaa') // 报错，未导出的方法不能在命名空间外部使用。
Utils.isStringForExport("sfsdf"); // true

namespace App {
	// 可以从别的命名空间导入方法，比如说Utils.isStringForExport方法名过长，则可以用import为其重命名并使用
	// import 也可以在命名空间外部使用。
	import isString = Utils.isStringForExport;
	isString("aaa"); // true
}

```

### 2. 模块

原先叫外部空间。

export import，如果说命名空间是在一个文件内的模块化，那外部空间就是文件与文件之间的模块化。

没啥好说的，和js一样。之前写过。
