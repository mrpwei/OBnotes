
## 接口interface

接口是一系列抽象方法的声明，是一些方法特征的集合。简单来说，接口的作用就是为这些类型命名和为你的代码或第三方代码定义契约。

接口跟类型别名类似，都是用来定义类型注解的，接口是用 `interface` 关键字来实现的，如下：

```TypeScript
interface A {
  readonly username: string;
  age: number;
}
let a: A = {
  username: 'xiaoming',
  age: 20
}
```

因为接口跟类型别名功能类似，所以接口也具备像索引签名，可调用注解等功能。

```TypeScript
interface A {
  [index: number]: number;
}
let a: A = [1, 2, 3];

interface A {
  (): void;
}
let a: A = () => {}
```

## 接口interface与别名type的区别

那么接口跟类型别名除了很多功能相似外，他们之间也是具备某些区别的。

1.  对象类型
2.  接口合并
3.  接口继承
4.  映射类型

第一个区别，类型别名可以操作任意类型，而接口只能操作对象类型。

第二个区别，接口可以进行合并操作。

```TypeScript
interface A {
  username: string;
}
interface A {
  age: number;
}
let a: A = {
  username: 'xiaoming',
  age: 20
}
```

第三个区别，接口具备继承能力。

```TypeScript
interface A {
  username: string
}
interface B extends A {
  age: number
}
let b: B = {
  username: 'xiaoming',
  age: 20
}
```

B这个接口继承了A接口，所以B类型就有了username这个属性。在指定类型的时候，b变量要求同时具备A类型和B类型。

第四个区别，接口不具备定义成接口的映射类型，而别名是可以做成映射类型的，关于映射类型的用法后面小节中会详细的进行讲解，这里先看一下效果。

```TypeScript
type A = {   // success
  [P in 'username'|'age']: string;
}
interface A {   // error
  [P in 'username'|'age']: string;
}
```