

## 什么是组合式API

组合式API是有别于选项式API的另一种新型写法，它*更适合编写复杂的应用*，假如我们要完成一个搜索功能，可能会有如下功能需要实现：

- 搜索提示列表
- 搜索结果列表
- 搜索记录列表

![05-01-搜索案例](http://qny.mrpwei.cc/uPic/05-01-搜索案例.png)

那么分别通过选项式和组合式是如何组织和实现代码的呢？可以发现选项式组织代码的时候，代码的跳跃性特别的强，对于后期维护特别的不方便；而组合式则是把相同功能的代码放到一起，功能独立，易于维护。

![05-02-选项式VS组合式](http://qny.mrpwei.cc/uPic/05-02-选项式VS组合式.png)

## setup方法与script_setup

在Vue3.1版本的时候，提供了setup方法；而在Vue3.2版本的时候，提供了script_setup属性。setup属性比setup方法对于操作组合式API来说会更加的简易。

```js
<template>
  <div>
    <h2>setup方法</h2>
    {{ count }}
  </div>
</template>

// Vue3.1
<script>
export default {
  name: 'Header',
  components: [],
  setup () {
    let count = 0;
    return {
      count
    }
  }
}
</script>

// Vue3.2
<script setup>
let count = 0;
</script>
```

setup方法需要把数据进行return后才可以在template标签中进行使用，而setup属性定义好后就可以直接在template标签中进行使用。

若要指定name，需引入`defineComponent`：

```js
<script setup>
import { defineComponent } from 'vue'

defineComponent({
    name: 'Header'
})
let count = 0;
</script>
```

## ref响应式

下面来学习一下，如何在组合式API中来完成数据的响应式操作，通过的就是`ref()`方法，需要从vue模块中引入这个方法后才可以使用。

```js
<script setup>
import { ref } from 'vue';
let count = ref(0);   // count -> { value: 0 }
//count += 1;   //✖
count.value += 1;   // ✔
</scirpt>
```

count数据的修改，需要通过`count.value`的方式，因为vue底层对响应式数据的监控必须是对象的形式，所以我们的count返回的并不是一个基本类型，而是一个对象类型，所以需要通过count.value进行操作。这种使用方式可能会增加我们的负担，可以通过Volar插件可以自动完成`.value`的生成，大大提高了使用效率。

现在count就具备了响应式变化，从而完成视图的更新操作。

`ref()`方法还可以关联原生DOM，通过标签的ref属性结合到一起，也可以关联到组件上。

```js
<template>
  <div>
    <h2 ref="elem">setup属性方式</h2>
  </div>
</template>

<script setup>
import { ref } from 'vue';
let elem = ref();
setTimeout(()=>{
  console.log( elem.value );   //拿到对应的原生DOM元素
}, 1000)
</script>
```

## 方法与计算属性

下面看一下在组合式API中是如何实现事件方法和计算属性的。

```js
<template>
  <div>
    <button @click="handleChange">点击</button>
    {{ count }}, {{ doubleCount }}
  </div>
</template>
<script setup>
import { computed, ref } from 'vue';
let count = ref(0);
let doubleCount = computed(()=> count.value * 2)
let handleChange = () => {
  count.value += 1;
};
</script>
```

方法直接定义成一个函数即可，计算属性需要引入computed方法。

## reactive与toRefs

`reactive()`方法是组合式API中另一种定义响应式数据的实现方式，它是对象的响应式。

```js
<template>
  <div>
    <h2>reactive</h2>
    {{ state.count }}
  </div>
</template>

<script setup>
import { reactive } from 'vue';
let state = reactive({
  count: 0,
  message: 'hi vue'
})
state.count += 1;
</script>
```

`reactive()`方法返回的本身就是一个state对象，那么在修改的时候就不需要.value操作了，直接可以通过`state.count`的方式进行数据的更改，从而影响视图的变化。

一般来说，`ref()`方法针对基本类型的响应式处理，而`reactive()`针对对象类型的响应式处理，当然还可以通过`toRefs()`方法把reactive的代码转换成ref形式。

```js
<template>
  <div>
    <h2>reactive</h2>
    {{ state.count }}, {{ count }}
  </div>
</template>

<script setup>
import { reactive, toRefs } from 'vue';

let state = reactive({
  count: 0
})
let { count } = toRefs(state);   //  let count = ref(0)
setTimeout(() => {
  //state.count += 1;
  count.value += 1;
}, 1000)

</script>
```