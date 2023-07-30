
## 枚举类型

枚举是组织收集有关联集合的一种方式，使代码更加易于阅读。其实简单来说枚举就是定义一组常量。

```TypeScript
enum Roles {
  SUPER_ADMIN,
  ADMIN = 3,
  USER
}
console.log( Roles.SUPER_ADMIN );   // 0
console.log( Roles.ADMIN );    // 3
console.log( Roles.USER );     // 4
```

枚举默认不给值的情况下，就是一个从0开始的数字，是可以自动进行累加的，当然也可以自己指定数值，后面的数值也是自动 累加的。

枚举也支持反向枚举操作，通过数值来找到对应的key属性，这样操作起来会非常的灵活。

```TypeScript
enum Roles {
  SUPER_ADMIN,
  ADMIN = 3,
  USER
}
console.log( Roles[0] );    // SUPER_ADMIN
console.log( Roles[3] );    // ADMIN
console.log( Roles[4] );    // USER
```

枚举给我们的编程带来的好处就是更容易阅读代码（因为数字没有语义，不利于理解代码）举例如下：

```TypeScript
if(role === Roles.SUPER_ADMIN){   // 更容易阅读
}
```

下面来看一下，如果定义成字符串的话，需要注意一些什么？

```TypeScript
enum Roles {
   SUPER_ADMIN = 'super_admin',
   ADMIN = 'admin',
   USER = 'user'
}
```

字符串形式是没有默认值的，而且不能做反向映射的。

枚举既可以作为值，也可以作为类型（即有有类型声明空间和变量声明空间）

```TypeScript
let a: Roles.ADMIN = Roles.ADMIN
```

## const枚举

在枚举的前面可以添加一个const关键字。

```TypeScript
const enum Roles {
  SUPER_ADMIN = 'super_admin',
  ADMIN = 'admin',
  USER = 'user'
}
console.log(Roles.ADMIN)

```

那么没有const关键字和有const关键字的区别是什么呢？主要区别在于编译的最终结果，const方式最终编译出来的就是一个普通字符串，并不会产生一个对象，更有助于性能的体现。

没有const编译成JS后：

![WWl1P4](http://qny.mrpwei.cc/uPic/WWl1P4.jpg)

有const编译成JS后：

![RNvyb4](http://qny.mrpwei.cc/uPic/RNvyb4.jpg)