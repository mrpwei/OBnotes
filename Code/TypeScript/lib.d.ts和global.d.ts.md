
## lib.d.ts

当你安装 TypeScript 时，会顺带安装一个 lib.d.ts 声明文件。这个文件包含 JavaScript 运行时以及 DOM 中存在各种常见的环境声明。

当我们使用一些原生JS操作的时候，也会拥有类型，代码如下：

```typescript
let body: HTMLBodyElement | null = document.querySelector('body')
let date: Date = new Date()
```

这里的`HTMLBodyElement`和`Date`都是TypeScript下自带的一些内置类型，这些类型都存放在lib这个文件夹下。

![09-01-lib.d.ts](http://qny.mrpwei.cc/uPic/09-01-lib.d.ts.png)

## global.d.ts

有时候我们也想扩展像lib.d.ts这样的声明类型，可以在全局下进行使用，所以TS给我们提供了global.d.ts文件使用方式，这个文件中定义的类型都是可以直接在全局下进行使用的，不需要模块导入。

```typescript
// global.d.ts
type A = string
```

```typescript
// 1_demo.ts
let a: A = 'hello'   // ✔
let b: A = 123       // ✖
```


关于*.d.ts文件，描述正确的是？

A：可以让JS文件在TS文件中进行使用  
B：可以通过declare关键字在*.d.ts文件中声明类型  
C：可以进行TS转JS的编译配置  
D：必须手动创建，不能自动生成

参考答案：

选项 AB ( TS转JS是通过tsconfig.json进行编译配置的；可以通过tsconfig.json中"declaration": true进行自动生成 )