## MVVM

使用 Vue 框架开发前端项目，最大的优势就是再也不用进行复杂的 DOM 操作了，我们只要关心数据的变化即可，Vue 框架会帮我们把复杂的 DOM 进行渲染，这背后都要归功于他的设计思想，即 MVVM 设计模式。

了解 MVVM 设计模式之前，有必要先了解一下 MVC 设计模式，MVVM 模式是在 MVC 模式基础上演变而来的。

最早的 MVC 设计模式是出现在后端开发中，主要目的就是让视图层与数据层分离，职责更加清晰，方便开发等等，例如：Spring MVC、[ASP.NET](http://ASP.NET) MVC 等等。

随着 Ajax 技术的流行，前后端分离开发越来越流行，前端需要处理复杂的视图与数据，迫使前端也急需一种设计模式来进行分层处理，所以 MVC 设计模式开始进入前端领域。

MVVM最早由微软提出来，它借鉴了桌面应用程序的MVC思想，在前端页面中，把Model用纯JavaScript对象表示，View负责显示，两者做到了最大限度的分离。把Model和View关联起来的就是ViewModel。ViewModel负责把Model的数据同步到View显示出来，还负责把View的修改同步回Model。MVVM 设计模式的核心思想就是不让 Model 和 View 这两层直接通信，而是通过 VM 层来连接。

![mvvm](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/82200b721acf4e849f9259d06506ed7b~tplv-k3u1fbpfcp-zoom-1.image)

![vue-mvvm](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6cb2365c32584c3f9625cadef189960c~tplv-k3u1fbpfcp-zoom-1.image)

ViewModel如何编写？需要用JavaScript编写一个通用的ViewModel，这样就可以复用整个MVVM模型了。

一个MVVM框架和jQuery操作DOM相比有什么区别？

我们先看用jQuery实现的修改两个DOM节点的例子：

```html
<p>Hello, <span id="name">Bart</span>!</p>
<p>You are <span id="age">12</span>.</p>
```

用jQuery修改name和age节点的内容：

```js
var name = 'Homer';
var age = 51;

$('#name').text(name);
$('#age').text(age);
```

如果我们使用MVVM框架来实现同样的功能，我们首先并不关心DOM的结构，而是关心数据如何存储。最简单的数据存储方式是使用JavaScript对象：

```js
var person = {
    name: 'Bart',
    age: 12
};
```

我们把变量person看作Model，把HTML某些DOM节点看作View，并假定它们之间被关联起来了。要把显示的name从Bart改为Homer，把显示的age从12改为51，我们并不操作DOM，而是直接修改JavaScript对象：

```js
person.name = 'Homer';
person.age = 51;
```

执行上面的代码，我们惊讶地发现，改变JavaScript对象的状态，会导致DOM结构作出对应的变化！这让我们的关注点从如何操作DOM变成了如何更新JavaScript对象的状态，而操作JavaScript对象比DOM简单多了！

这就是MVVM的设计思想：关注Model的变化，让MVVM框架去自动更新DOM的状态，从而把开发者从操作DOM的繁琐步骤中解脱出来！

MVVM 设计模式比 MVC 模式的优势：

-   ViewModel 能够观察到数据的变化，并对视图对应的内容进行自动更新
-   ViewModel 能够监听到视图的变化，并能够通知数据发生变化

虽然最早提出 MVVM 模式的是 Angular.js，但是 Vue 把 MVVM 设计模式发扬光大了，Vue 也成为了当下最主流的前端框架之一。

Vue 官网上的一段话：虽然没有完全遵循 MVVM 模型，但是 Vue 的设计也受到了它的启发。因此在文档中经常会使用 vm (ViewModel 的缩写) 这个变量名表示组件实例。

MVVM 模型中 M 和 V 不能直接操作，需要 VM 层加持。但 Vue 比较灵活，可以直接去操作原生 DOM，也就是直接去操作 V 层。

参考：
[MVVM - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/1022910821149312/1108898947791072)

## optionsAPI

Options API编程风格通过定义methods，computed，watch，data等属性与方法，共同处理页面逻辑：

```
let vm = createApp({
  methods: {},
  computed: {},
  watch: {},
  data() {},
  mounted() {},
});
```
<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a2fe1010e8414708b4e05d35eb96e0b5~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="30%" />

这种写法的优点：

-   只有一个参数，不会出现参数顺序的问题，随意调整配置的位置
-   非常清晰，语法化特别强
-   非常适合添加默认值

缺点：
- 随着组件功能的增大,关联性会大大降低,组件的阅读和理解难度会增加;
- 调用使用this,但逻辑过多时this会出现问题,比如指向不明等;
- 其本身并不是有效的js代码，我们在使用options API的时候，需要确切了解具体可以访问到哪些属性。访问到的当前属性的行为在后台，Vue需要将此属性转换为工作代码，无法进行代码补全和类型检查，因此在使用相关属性时，造成了一定弊端。

Composition API是组件根据逻辑功能来组织的，一个功能所定义的所有 API 会放在一起（更加的高内聚，低耦合），即使项目很大，功能很多，我们都能快速的定位到这个功能所用到的所有API

<img src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7ee68df3225f499ba5deaa73ebade6b8~tplv-k3u1fbpfcp-watermark.image?" alt="image.png" width="50%" />

优点:
- 其代码更易读，更易理解和学习，没有任何幕后操作
- Composition API的好处不仅仅是以不同的方式进行编码，更重要的是对于代码的重用
- 不受模板和组件范围的限制，也可以准确的知道我们可以使用哪些属性
- 由于幕后没有什么操作，所以编辑器可以帮助我们进行类型检查和建议

两者对比：

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c00f2e90759e4a94aac6cb2e0b482c63~tplv-k3u1fbpfcp-watermark.image?)

总结：
- 在逻辑组织和逻辑复用方面，Composition API是优于Options API
- 因为Composition API几乎是函数，会有更好的类型推断。
- Composition API对 tree-shaking 友好，代码也更容易压缩
- Composition API中没有对this的使用，减少了this指向不明的情况
- 如果是小型组件，可以继续使用Options API，也是十分友好的