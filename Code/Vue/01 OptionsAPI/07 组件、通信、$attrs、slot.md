## 组件

组件是带有名词的可复用实例，通常一个应用会以一颗嵌套的组件树的形式来组织，比如：页头、侧边栏、内容区等组件。

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1b0a6a56a1354344be746f93df8f692d~tplv-k3u1fbpfcp-watermark.image?)


![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f5d22bcac23f4ec390ff0caa2b03f670~tplv-k3u1fbpfcp-watermark.image?)

组件命名使用单词第一个字母大写或短线连接单词：LeftPanel||left-panel，调用组件使用短线式left-panel。不要使用一个单词来定义Vue组件名，容易和html自带标签冲突。

```html
<div id="app">
    <my-head></my-head>
</div>
<script>
    let app = Vue.createApp({});
    
    app.component("my-head", {
        template: `
            <header>logo</header>
            <div>{{ message }}</div>
            <ul>
                <li>音乐</li>
                <li>视频</li>
                <li>文章</li>
            </ul>`,
        data() {
          return {
            message: "hello",
          };
        },
    });

    let vm = app.mount("#app");
</script>
```

根组件默认的template就是`id="app"`的div里面的内容，一般不用显式写出。

### 全局组件与局部组件

全局组件app下都可以用，局部组件只有注册该组件的当前组件可以使用。

全局组件：

```html
<div id="app">
    <my-logo></my-logo>
    <my-head></my-head>
</div>
<script>
    let app = Vue.createApp({});

    let Head = {
        template: `
          <header>
          <div>{{ message }}</div>
          <ul>
              <li>音乐</li>
              <li>视频</li>
              <li>文章</li>
          </ul>
          </header>`,
        data() {
          return {
            message: "hello",
          };
        },
    };

    let Logo = {
        template: `<h2>logo</h2>`,
    };

    app.component("my-head", Head);
    app.component("my-logo", Logo);

    let vm = app.mount("#app");
</script>
```

局部组件：

```html
<div id="app">
    <my-head></my-head>
</div>
<script>
    let app = Vue.createApp({});

    let MyLogo = {
        template: `<h2>logo</h2>`,
    };

    let MyHead = {
        template: `
          <header>
          <my-logo></my-logo>
          <ul>
              <li>音乐</li>
              <li>视频</li>
              <li>文章</li>
          </ul>
          </header>`,
        //局部组件
        components: {
          MyLogo,
        },
    };

    app.component("my-head", MyHead);

    let vm = app.mount("#app");
</script>
```


## 组件之间是如何进行互相通信的

上一个小节中，我们了解了组件是可以组合的，那么就形成了父子组件，父子组件之间是可以进行通信的， 那么为什么要通信呢？主要是为了让组件满足不同的需求。

### 父子通信

最常见的通信方式就是父传子，或者子传父，那么父传子通过props实现，而子传父则通过emits自定义事件实现。

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/90972caac80d4defbeca985e60a88fa7~tplv-k3u1fbpfcp-watermark.image?)

```html
<div id="app">
    <my-head title="hello" count="10"></my-head>
</div>
<script>
    let app = Vue.createApp({
        data(){
            return {
            }
        }
    })
    app.component('MyHead', {
        props: ['title', 'count'],
        template: `
        <header>
          <div>{{ title }}, {{ count }}</div>
    	</header>`,
        mouted(){
        	this.$emit('custom-click', 'MyHead Data')
    	}
    });
    let vm = app.mount('#app');
</script>
```

和响应式数据关联，子组件能拿到更新后的值：

```html
<div id="app">
    <my-head :count="count" @custom-click="handleClick"></my-head>
</div>
<script>
    let app = Vue.createApp({
        data(){
            return {
                count: 10
            }
        },
        methods: {
            handleClick(data){
              console.log(data);
            }
        }
    })
    app.component('MyHead', {
        props: ['count'],
        //注册
        emits: ['custom-click'], 
        template: `
        <header>
          <div>{{ count }}</div>
          <h2>logo</h2>
          <ul>
            <li>首页</li>
            <li>视频</li>
            <li>音乐</li>
    	  </ul>
    	</header>`,
        mouted(){
            //第一个参数是自定义事件名，第二个参数是传递的值
            this.$emit('custom-click', 'MyHead Data')
    	}
    });
    let vm = app.mount('#app');
</script>
```

定义props的时候推荐使用对象写法限定数据类型、设置默认值等。

- 组件通信的props是可以定义类型的，在运行期间会进行检测
- 组件之间的数据是**单向流动**的，子组件不能直接修改传递过来的值
- 但是有时候也需要数据的双向流动，可利用v-model来实现

子组件不能直接修改传递过来的值，但是可以在data中复制一份，修改复制后的数据(因为data是在props传值后再调用的)。

```
props: {
    'count': {
        type: Number
    }
},
data() {
    return {
        myCount: this.count
    }
}
```

v-model示例：
[代码片段](https://code.juejin.cn/pen/7180325670667419709)


## 组件的属性与事件是如何进行处理的

有时候组件上的属性或事件并不想进行组件通信，那么Vue是如何处理的呢？

- 属性不通过props接收的话，会直接以普通html属性挂载到组件容器上
- 事件也是如此，默认会直接挂载到组件容器上。
- 可通过 inheritAttrs 选项阻止这种行为，通过指定这个属性为false，可以避免组件属性和事件向容器传递。
- 可通过 $attrs 内置语法，给指定元素传递属性和事件，代码如下：

```html
<div id="app">
    <my-head title="hello world" class="box" @click="handleClick"></my-head>
</div>
<script>
    let app = Vue.createApp({
      data(){
        return {
        }
      },
      methods: {
        handleClick(ev){
          console.log(ev.currentTarget);
        }
      }
    })
    app.component('MyHead', {
      template: `
        <header>
          <h2 v-bind="$attrs">logo</h2>
          <h2 v-bind:title="$attrs.title">logo</h2>
          <ul v-bind:class="$attrs.class">
            <li>首页</li>
            <li>视频</li>
            <li>音乐</li>
          </ul>
        </header>
      `,
      mounted(){
        console.log( this.$attrs );   // 也可以完成父子通信操作
      },
      inheritAttrs: false   // 阻止默认的属性传递到容器的操作
    });
    let vm = app.mount('#app');
</script>
```

# 插槽

在前面的小节中，我们学习了组件之间的通信，让组件之间实现了不同的需求，我们通过给组件添加不同的属性来实现。那么在Vue中如何去传递不同的组件结构呢？这就涉及到了组件内容的分发处理。

在Vue中是通过插槽slot方式来进行分发处理的，Vue 实现了一套内容分发的 API，这套 API 的设计灵感源自 Web Components 规范草案，将 `<slot>` 元素作为承载分发内容的出口。

```html
<div id="app">
    <my-head>
    	<p>logo</p>
    </my-head>
</div>
<script>
    let app = Vue.createApp({
      data(){
        return {
          message: 'hello'
        }
      }
    })
    app.component('MyHead', {
      data(){
        return {
        };
      },
      template: `
        <header>
          <slot></slot>
        </header>`,
    });
    let vm = app.mount('#app');
</script>
```

组件内的结构，即`<p>logo</p>`会被分发到`<slot></slot>`所在的区域。

## 内容分发与插槽的注意点

- 渲染作用域 -> 插槽只能获取当前组件的作用域
- 具名插槽 -> 处理多个插槽的需求，通过`v-slot`指令实现，简写为`#`
- 作用域插槽 -> 希望插槽能访问子组件的数据

完整代码如下：

```html
<div id="app">
    <my-head>
      <template v-slot:title>
        <p>logo, {{ message }}, {{ count }}</p>
      </template>
      <template #list="{ list }"> //作用域插槽获取、解构数据
        <ul>
          <li v-for="item in list">{{ item }}</li>
        </ul>
      </template>
    </my-head>
  </div>
  <script>
    let app = Vue.createApp({
      data(){
        return {
          message: 'hello'
        }
      }
    })

    app.component('MyHead', {
      data(){
        return {
          count: 123,
          list: ['首页', '视频', '音乐']
        };
      },
      template: `
        <header>
          <slot name="title"></slot>
          <hr>
          <slot name="list" :list="list"></slot> //作用域插槽定义数据
        </header>
      `,
    });
    let vm = app.mount('#app');
</script>
```

`$attrs`也可以实现组件之间的间接通信。