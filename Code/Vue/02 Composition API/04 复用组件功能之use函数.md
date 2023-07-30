
为了更好的组合代码，可以创建统一规范的use函数，从而抽象可复用的代码逻辑。利用use函数可以达到跟mixin混入一样的需求，并且比mixin更加强大。这只是一套代码规范，不是新功能。

```js
// useCounter.js
import { computed, ref } from 'vue';
function useCounter(num){
  let count = ref(num);
  let doubleCount = computed(()=> count.value * 2 );
  return {
    count,
    doubleCount
  }
}

export default useCounter;
```

```js
<template>
  <div>
    <h2>use函数</h2>
    <div>{{ count }}, {{ doubleCount }}</div>
  </div>
</template>

<script setup>
import useCounter from '@/compotables/useCounter.js'
let { count, doubleCount } = useCounter(123);
setTimeout(()=>{
  count.value += 1;
}, 2000);
</script>
```

通过useCounter函数的调用，就可以得到内部return出来的对象，这样就可以在.vue文件中进行功能的使用，从而实现功能的复用逻辑。

内置的这些use函数只能在CompositionAPI中使用。