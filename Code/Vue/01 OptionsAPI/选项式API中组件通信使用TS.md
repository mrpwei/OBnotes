
![W06oBi](http://qny.mrpwei.cc/uPic/W06oBi.png)

## 父子通信使用TS

主要利用的方案是，`props + PropType`模式，首先`props`属性可以直接采用Vue的方式来完成TS的类型注解。

```js
// 父组件
<template>
  <div>
    <h2>Hello 02</h2>
    <my-head :count="count" :list="list" @get-data="getData"></my-head>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import MyHead from './03_MyHead.vue'
export default defineComponent({
  data(){
    return {
      count: 0,
      list: [
        {username: 'xiaoming', age: 20}
      ]
    }
  },
  components: {
    MyHead
  },
  methods: {
    getData(payload: string){
      console.log( payload );
    }
  }
});
</script>
```

```vue
<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
  props: {
    count: [Number, String]
  }
})
</script>
```

这样在父子通信的时候，count只能传递number或string这两种类型，如果传递其他类型的话，TS就会进行提示。

那么如果我们不通过Vue的方式进行提示的话，而是采用TS的接口或别名进行类型注解该如何做到呢？那么就需要Vue给我们提供的一个类型`PorpType`来实现了。

```vue
<script lang="ts">
import { defineComponent } from 'vue'
import type { PropType } from 'vue'
interface List {
  username: string
  age: number
}
export default defineComponent({
  props: {
    count: [Number, String],
   	list: Object as PropType<List[]>
  }
})
</script>
```

可以看到，`PropType`是需要引入的，再配合类型断言和泛型的使用方式来进行实现的。`import type {PropType} from 'vue'`中的type可以省略，但是为了表明是数据类型，建议加上。

## 子父通信使用TS

主要利用的方案是`emits + 对象方式`的方案，首先我们要回忆一下子父通信是如何做到的？利用emits属性来实现的，当调用`this.$emit()`来触发自定义事件，但是这种模式的方式，并不会自动进行类型推断的，毕竟调用的是$emit()方法，而不是我们对应的事件函数。

那么可以使用emits进行主要的类型注解的方式来进行自定义事件调用时候传递的事件参数类型，所以emits需要把数组的写法改成对象的写法，这样才能进行函数参数类型的限定。

```js
// 子组件
<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
  //emits: ['get-data'],    // 限定不了
  emits: {
    'get-data'(payload: string){ return payload.length > 0 }   // 可以限定
  },
  
  mounted(){
    this.$emit('get-data', 'hello');  // ✔
  }
})
</script>
```
