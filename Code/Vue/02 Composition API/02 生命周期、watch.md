

## 生命周期钩子函数

选项式API与组合式API中生命周期对比：

![05-03-生命周期对比](http://qny.mrpwei.cc/uPic/05-03-生命周期对比.png)

具体的区别如下：

- 组合式api中没有`beforeCreate`和`created`这两个生命周期，因为本身组合式api默认就在created当中，直接定义完响应式数据后就可以直接拿到响应式数据，所以不需要再有beforeCreate和created这两个钩子
- 组合式的钩子前面会有一个on，类似于事件的特性，表示可以多次重复调用，这样逻辑就不用都写到一个地方，而是可以抽离到不同的函数中。

```js
<template>
  <h2>生命周期</h2>
  <div ref="elem">{{ count }}</div>
</template>

<script setup>
import { onBeforeMount, onMounted, ref } from "vue";

let count = ref(0);
let elem = ref();

onBeforeMount(() => {
  console.log(count.value);
  console.log(elem.value);
});
onMounted(() => {
  console.log(count.value);
});
onMounted(() => {
  console.log(elem.value);
});
</script>
```

## watch与watchEffect

这里先说一下watchEffect的用法，它比watch更强大。为了根据响应式状态自动应用和重新应用副作用，我们可以使用 watchEffect 函数。它立即执行传入的一个函数，同时响应式追踪其依赖，并在其依赖变更时重新运行该函数。

watchEffect常见特性：

- 初始化时会触发一次，然后所依赖的数据发生改变的时候，才会再次触发
- 触发的时机是数据响应后，DOM更新前，通过`flush: 'post'`可修改成DOM更新后进行触发

```js
<template>
  <h2>watchEffect</h2>
  <div ref="elem">{{ count }}</div>
</template>

<script setup>
import { watchEffect, ref } from "vue";

let count = ref(0);
let elem = ref();

watchEffect(
  () => {
    console.log(count.value);
    if (elem.value) {
      console.log(elem.value.innerHTML);
    }
  },
  { flush: "post" }
);

setTimeout(() => {
  count.value += 1;
}, 2000);
</script>
```

- 返回结果是一个stop方法，可以停止watchEffect的监听

```js
<script setup>
import { watchEffect, ref } from "vue";

let count = ref(0);
let elem = ref();

const stop = watchEffect(
  () => {
    console.log(count.value);
  },
  { flush: "post" }
);

setTimeout(() => {
  stop();
}, 1000);

setTimeout(() => {
  count.value += 1;
}, 2000);
</script>
```

- 提供一个形参，其初始化时不触发，更新前和卸载前触发。形参主要就是用于清除上一次的行为（如ajax请求、定时器）。

```js
<template>
  <div>
    <h2>watchEffect</h2>
    <div>{{ count }}</div>
  </div>
</template>

<script setup>
import { ref, watchEffect } from 'vue';
let count = ref(0);

watchEffect((cb)=>{
  console.log(count.value);
  cb(()=>{
    //更新前触发和卸载前触发，目的：清除上一次的行为(停止上一次的ajax、清除上一次的定时器)
    console.log('before update');
  })
})

setTimeout(()=>{
  count.value += 1;
}, 2000)
</script>
```

再来看一下watch侦听器的使用方式，如下：

```js
<script setup>
import { ref, watch } from 'vue';
let count = ref(0);
watch(count, (newVal, oldVal) => {
  console.log(newVal, oldVal);
})
setTimeout(() => {
  count.value = 1;
}, 2000)
</script>
```

[[04 计算属性、监听器、条件渲染与列表渲染#监听器watch|optionsAPI中的watch]]

那么watch与watchEffect的区别是什么呢？

以下情况考虑用watch：
- 懒执行副作用，即不需要初始化时触发，只在数据改变的时候触发
- 更具体地说明什么状态应该触发侦听器重新运行，即只监控某一个值，而不是像watchEffect一样可以依赖多个值，其中某一个值变化就会触发。
- 需要访问侦听状态变化前后的值
