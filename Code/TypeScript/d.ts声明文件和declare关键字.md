
## d.ts声明文件

在 TypeScript 中以 `.d.ts` 为后缀的文件，我们称之为 TypeScript 声明文件。它的主要作用是描述 JavaScript 模块内所有导出接口的类型信息。

当我们开发了一个模块，我们需要让模块既可以适配JS项目，又可以适配TS项目，那么就可以利用`.d.ts`声明文件来实现，这样就可以让我们的JS模块在TS环境下进行使用了，而类型空间就交给声明文件来处理吧。

```javascript
// 01_demo.js
function foo(n) {
    console.log(n);
}
exports.foo = foo;
```

```typescript
// 01_demo.d.ts
export declare function foo(n: number): void
```

这里可以看到`declare`这个关键词，就是在声明文件中进行类型定义的，这个只是用于定义类型，不会产生任何功能实现，具体的功能是由`01_demo.js`文件来实现的。

这样定义好了`01_demo.js`所配套的声明文件后，那么就可以把这个`01_demo.js`文件在TS文件中进行导入，并且正常的进行使用。

```typescript
// 02_demo.ts
import { foo } from './01_demo'
foo(123)   // ✔
foo('hello')  // ✖
```

可以看到当往函数中传递不正确类型的时候，声明文件就会起作用，提示类型错误的信息。

声明文件总结来说，就是可以让我们的JS文件在TS中进行使用，从而适配JS和TS两个环境。

不过自己手写声明文件是比较麻烦的，所以当我们用TS去编写代码的时候，可以利用`tsconfig.json`文件自动创建转换后的声明文件。

```json
// tsconfig.json
"declaration": true,   // 打开注释后，自动生成.d.ts文件   
```

这样当代码多了，我们也不用担心编写声明文件的问题了，让TS自动帮我们去生成就好了。

