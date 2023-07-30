
## 指令系统

指令系统就是通过自定义属性实现的一套功能，也是声明式编程的体现。

通常在标签上添加 `v-` 字样，常见的指令有：

- v-bind -> 操作标签属性，可通过 `:` 简写
- v-on -> 操作事件，可通过 `@` 简写

```html
<div id="app">
  <p :title="message">这是一个段落</p>
  <button @click=" message = 'hi' ">点击</button>
</div>
{{ message }}
<script>
  let vm = Vue.createApp({
    data() {
      return {
        message: "hello",
      };
    },
  }).mount("#app");
</script>
```

## 方法

如何添加事件方法?通过 methods 选项 API 实现，并且 Vue 框架已经帮我们帮事件传参机制处理好了。

```html
<div id="app">
  <button @click="toClick($event, 123)">点击</button>
</div>
<script>
  let vm = Vue.createApp({
    methods: {
      toClick(ev, num) {},
    },
  }).mount("#app");
</script>
```

- `@click="fun"`
不带括号、不写实参的fun默认传event (事件对象)

- `@click="fun(value)"`
只要加括号，无论是否传值，都属于传实参给函数，event (事件对象)就接收不到。
如果需要实参、又需要event (事件对象)，就需要手动传入 event (事件对象)，如下：
`@click="fun($event, value)"`

## class样式

操作元素的 class 列表和内联样式是数据绑定的一个常见需求。因为它们都是 attribute，所以我们可以用 `v-bind` 处理它们：只需要通过表达式计算出字符串结果即可。不过，字符串拼接麻烦且易错。因此，在将 `v-bind` 用于 `class` 和 `style` 时，Vue.js 做了专门的增强。表达式结果的类型除了字符串之外，还可以是对象或数组。

-   字符串
-   数组
-   对象

```js
//<div :class="myClass">hello</div>
//<div :style="myStyle">world</div>

let vm = Vue.createApp({
  data() {
    return {
      myClass1: "box box2",
      myClass2: ["box", "box2"],
      myClass3: { box: true, box2: true },
      myStyle1: "background: red; color: white;",
      myStyle2: ["background: red", "color: white"],
      myStyle3: { background: "red", color: "white" },
    };
  },
}).mount("#app");
```

数组和对象的形式要比字符串形式更加的灵活，也更容易控制变化。比如如果使用字符串class，2秒后要修改删除样式，就只能截取字符串，而使用数组就只需要pop。