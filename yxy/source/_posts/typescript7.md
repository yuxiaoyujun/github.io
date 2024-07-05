---
title: 'typescript总结笔记（7）配置文件、命令行、双斜线指令'
date: 2023-03-11 19:44:13
tags: 
  - typescript
categories:
  - 程序员的自我修养
---
  <meta name="referrer" content="no-referrer">

## 一、配置文件 `tsconfig.json`

哎呀，懒得写了，反正常用配置就那几个

```typescript
{
	"include": ["src/**/*.ts"],
  "exclude": ["dist/*",'node_modules']
	"files": [
		"string.d.ts"
	],
	"compilerOptions": {
		"target": "es2016" /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */,
		"module": "commonjs" /* Specify what module code is generated. */,
		"rootDir": "./src" /* Specify the root folder within your source files. */,
		"moduleResolution": "node" /* Specify how TypeScript looks up a file from a given module specifier. */,
		// "baseUrl": "./",       
		// "outFile": "./build/page.js",                                  /* Specify a file that bundles all outputs into one JavaScript file. If 'declaration' is true, also designates a file that bundles all .d.ts output. */
		"outDir": "./dist" /* Specify an output folder for all emitted files. */,
		"esModuleInterop": true /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables 'allowSyntheticDefaultImports' for type compatibility. */,
		"forceConsistentCasingInFileNames": true /* Ensure that casing is correct in imports. */,
		"strict": true /* Enable all strict type-checking options. */,
		"skipLibCheck": true /* Skip type checking all .d.ts files. */,
		"experimentalDecorators": true
	}
}

```

1. `"compilerOptions"`：这是最重要的部分，用于配置 TypeScript 编译器的选项。以下是一些常见的选项：
   - `"target"`：指定目标 ECMAScript 版本。常见的选项包括 `"ES5"`、`"ES6"`、`"ESNext"` 等。
   - `"module"`：指定模块系统，常见的选项包括 `"CommonJS"`、`"ES6"`、`"UMD"`、`"AMD"` 等。
   - `"outDir"`：指定编译输出目录。
   - `"strict"`：启用严格模式，包括严格的类型检查和空检查。
   - `"esModuleInterop"`：启用 ES 模块的互操作性。
   - `"jsx"`：指定 JSX 支持，常见选项包括 `"react"` 和 `"preserve"`。
2. `"include"` 和 `"exclude"`：这两个选项用于指定哪些文件应该包含在编译中，以及哪些文件应该排除在外。它们可以使用通配符模式来匹配文件。
3. `"files"`：如果不使用 `"include"` 和 `"exclude"`，可以使用 `"files"` 选项明确列出要编译的文件。
4. `"extends"`：可以继承其他 tsconfig.json 文件的设置，以便在多个项目中共享通用配置。
5. `"includeDeclarations"`：如果需要生成 `.d.ts` 声明文件，可以启用此选项。
6. `"types"` 和 `"typeRoots"`：用于管理类型声明文件的依赖和搜索路径。
7. `"baseUrl"` 和 `"paths"`：用于配置模块解析路径别名，便于导入模块。
8. `"rootDir"` 和 `"composite"`：用于配置项目的根目录和启用项目引用。

## 二、命令行

其实常用命令一样就那几个

1. **编译 TypeScript 文件：**

   ```
   tsc file.ts
   ```

   这将编译指定的 TypeScript 文件（`file.ts`）并生成对应的 JavaScript 文件。

2. **编译整个项目：**

   ```bash
   tsc
   ```

   如果你在项目的根目录中有一个 `tsconfig.json` 配置文件，运行 `tsc` 命令将会编译整个 TypeScript 项目，根据配置文件中的设置进行编译。

3. **使用不同的配置文件编译项目：**

   ```bash
   tsc -p customconfig.json
   ```

   这将使用指定的自定义配置文件（`customconfig.json`）来编译 TypeScript 项目。

4. **监视模式：**

   ```bash
   tsc -w
   ```

   使用 `-w` 选项启动 TypeScript 编译器的监视模式，它会自动监视文件的更改并在保存时重新编译。

5. **生成声明文件（.d.ts）：**

   ```bash
   tsc --declaration
   ```

   使用 `--declaration` 选项生成 TypeScript 声明文件（`.d.ts`），这对于发布 TypeScript 库非常有用。

6. **生成 Source Maps：**

   ```bash
   tsc --sourceMap
   ```

   使用 `--sourceMap` 选项生成 JavaScript 的 Source Map 文件，以便在调试时将编译后的 JavaScript 映射回 TypeScript。

7. **指定输出目录：**

   ```bash
   tsc --outDir dist
   ```

   使用 `--outDir` 选项指定编译输出文件的目录。

8. **删除编译结果：**

   ```bash
   tsc --clean
   ```

   使用 `--clean` 选项删除先前的编译结果。

## 三、双斜线指令

以下是一些常见的双斜线指令示例：

1. **`// @ts-ignore`：**

   这个指令告诉 TypeScript 编译器忽略下一行代码的类型检查错误。

   ```typescript
   // @ts-ignore
   const x: number = 'hello'; // No error reported
   ```

2. **`// @ts-check`：**

   这个指令告诉 TypeScript 编译器检查下一行代码的类型，并在存在错误时报告错误。

   ```typescript
   // @ts-check
   const x: number = 'hello'; // Error: Type 'string' is not assignable to type 'number'
   ```

3. **`// @ts-nocheck`：**

   这个指令告诉 TypeScript 编译器在接下来的代码中禁用类型检查，直到遇到下一个指令（如`@ts-check`）。

   ```typescript
   // @ts-nocheck
   const x: number = 'hello'; // No error reported
   ```

4. **`// @ts-expect-error` 和 `// @ts-expect-error 1234`：**

   这个指令告诉 TypeScript 编译器期望在接下来的代码中报告错误。你可以提供一个错误代码（如 `1234`）来指定期望的错误。

   ```typescript
   // @ts-expect-error
   const x: number = 'hello'; // Error expected
   ```

5. **JSDoc**

   TypeScript 直接处理 JS 文件时，如果无法推断出类型，会使用 JS 脚本里面的 JSDoc 注释。

   我没用过，先不写了。用到了看官网吧。
