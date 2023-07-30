## 引入Vue

```JavaScript
const app = Vue.createApp({
    data() { // data:function () {}的ES6写法
        return {
            products: 'socks'
        }
    }
})
```

```HTML
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Vue Mastery</title>
    <!-- Import Styles -->
    <link rel="stylesheet" href="./assets/styles.css" />
    <!-- Import Vue.js -->
    <script src="https://unpkg.com/vue@3.0.0-beta.12/dist/vue.global.js"></script>
  </head>
  <body>
    <div id="app">
      <h1>Mart</h1>
      <p>{{ products }}</p>
    </div>

    <!-- Import Js -->
    <script src="./main.js"></script>

    <!-- Mount dom to vueApp -->
    <script>
      const mountedApp = app.mount('#app')
    </script>

  </body>
</html>
```

## 遍历v-for

-   v-for遍历数组： (value, index）    
-   v-for遍历对象： (value, key, index)

### 改变原数组的方法

- 变异方法（mutation method）会改变原数组
	-   pop()
	-   push()
	-   shift()
	-   unshift()
	-   splice()
	-   sort()
	-   reverse()
- 非变异方法（non-mutation method）不会改变原始数组，而是返回一个新数组。
	- filter()
	- concat()
	- slice()

对于这些方法，想要Vue自动更新视图，可以使用新数组替换原来的数组。

Vue在监测到数据变化时，并不是直接重新渲染整个列表，而是最大化复用DOM元素。在替换的数组中，相同的元素项不会被重新渲染，因此不用担心性能问题。

### 过滤与排序

有时想要显示一个数组经过过滤或排序后的结果，但不实际改变原始数据，这种情况下可以创建一个计算属性来返回过滤或排序后的数组。

```javascript
<li v-for="n in evenNumbers">{{ n }}</li>

data() {
  return {
    numbers: [1,2,3,4,5]
  }
},
computed: {
  evenNumber() {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

