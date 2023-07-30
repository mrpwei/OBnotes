
## 跨组件通信方案

正常情况下，我们的组件通信是需要一级一级的进行传递，通过父子通信的形式，那么如果有多层嵌套的情况下，从最外层把数据传递给最内层的组件就非常的不方便，需要一级一级的传递下来，那么如何才能方便的做到跨组件通信呢？

可以采用Provide 和 inject 依赖注入的方式来完成需求，代码如下：

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2f58bf46aa47434aba4895c9c3830d74~tplv-k3u1fbpfcp-watermark.image?)


```js
// provide.vue
<script>
export default {
    provide(){
        return {
            message: 'hello provide', //传递数据
            count: this.count,        //传递响应式数据
            getInfo(data){            //获取数据
                console.log(data);
            }
        }
    }
}
</script>

// inject.vue
<template>
<div>
    hello inject, {{ message }}, {{ count }}
 </div>
</template>

<script>
export default {
    inject: ['message', 'getInfo', 'count'],
    mounted(){
        this.getInfo('hello inject');
    }
}
</script>
```

## Provide与Inject注意点

- 保证数据是单向流动的，从一个方向进行数据的修改
- 如果要传递响应式数据，需要把provide改造成工厂模式发送数据

```js
export default {
    //普通模式，无法传递响应式数据
    //provide: {
    //    message: 'hello provide',
    //    getInfo(data) {
    //        console.log(data);
    //    }
    //}
    
    //工厂模式，可以传递响应式数据
    provide(){
        return {
            message: 'hello provide', //传递数据
            count: this.count,        //传递响应式数据
            getInfo(data){            //获取数据
                console.log(data);
            }
        }
    }
}
```