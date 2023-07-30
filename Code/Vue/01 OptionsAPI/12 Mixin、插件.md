
## Mixin混入

本小节我们来了解一下mixin混入，它是选项式API的一种复用代码的形式，可以非常灵活的复用功能。

```js
// mymixin.js
const myMixin = {
  data(){
    return {
      message: '复用的数据'
    }
  },
  computed: {
    message2(){
      return '复用的数据2'
    }
  }
};
export {
  myMixin
}
// mymixin.vue
<template>
  <div>
    <h2>mixin混入</h2>
    <div>{{ message }}</div>
    <div>{{ message2 }}</div>
  </div>
</template>
<script>
  import { myMixin } from '@/mymixin.js'
  export default {
    mixins: [myMixin]
  }
</script>
```

这样就可以直接在.vue中使用这些混入的功能。当然这种方式是局部混入写法，也可以进行全局混入的写法，代码如下：

```javascript
// main.js
import { myMixin } from '@/mymixin.js'
app.mixin(myMixin)
```

mixin存在的问题也是有的，那就是不够灵活，不支持传递参数，这样无法做到差异化的处理，所以目前比较推荐的复用操作还是选择使用组合式API中的use函数来完成复用的逻辑处理。后面章节我们会学习到这种组合式API的应用。


## 插件的概念及插件的实现

插件是自包含的代码，通常向 Vue 添加全局级功能。例如：全局方法、全局组件、全局指令、全局mixin等等。基于Vue的第三方模块都是需要通过插件的方式在Vue中进行生效的，比如：Element Plus、Vue Router、Vuex等等。

```js
// myplugin.js
import * as http from '@/http.js'
export default {
  install(app, options){
    console.log(options);  // {info: '配置信息'}
    app.config.globalProperties.$http = http;
    app.directive('auth', (el, binding) => {
      let auths = ['edit', 'delete'];
      let ret = auths.includes(binding.value);
      if(!ret){
        el.style.display = 'none';
      }
    });
    app.component('my-head', {
      template: `<div>hello myhead</div>`
    })
  }
}
// main.js 让插件生效
import myplugin from './myplugin.js'
app.use(myplugin, { info: '配置信息' })
```

可以看到，让插件生效的语法为`app.use`，这样就可以跟Vue结合到一起，所以插件就可以独立出去，成为第三方模块。