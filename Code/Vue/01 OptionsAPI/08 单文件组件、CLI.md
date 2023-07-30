
Vue 单文件组件（又名 `*.vue` 文件，缩写为 SFC）是一种特殊的文件格式，它能够将 Vue 组件的模板、逻辑与样式封装在单个文件中。

### 为什么要使用 SFC

使用 SFC 必须使用构建工具，但作为回报带来了以下优点：

- 使用熟悉的 HTML、CSS 和 JavaScript 语法编写模块化的组件
- [让本来就强相关的关注点自然内聚](https://cn.vuejs.org/guide/scaling-up/sfc.html#what-about-separation-of-concerns)
- 预编译模板，避免运行时的编译开销
- 模块化图片、css样式等，使得这些资源可以以模块的形式import
- [组件作用域的 CSS](https://cn.vuejs.org/api/sfc-css-features.html)
- [在使用组合式 API 时语法更简单](https://cn.vuejs.org/api/sfc-script-setup.html)
- 通过交叉分析模板和逻辑代码能进行更多编译时优化
- [更好的 IDE 支持](https://cn.vuejs.org/guide/scaling-up/tooling.html#ide-support)，提供自动补全和对模板中表达式的类型检查
- 开箱即用的模块热更新 (HMR) 支持

### 如何支持SFC

可通过项目脚手架来进行支持，Vue支持Vite脚手架和Vue CLI脚手架。这里我们先来介绍Vue CLI的基本使用方式。

```shell
# 安装
sudo npm install -g @vue/cli
# 创建项目
vue create vue-study
# 选择default
default (babel, eslint)
# 启动脚手架
npm run serve
```

通过`localhost:8080`进行访问。

### 脚手架文件的组成

- src/main.js -> 主入口模块
- src/App.vue -> 根组件
- src/components -> 组件集合
- src/assets -> 静态资源

### 单文件的代码组成

- template -> 编写结构
- script -> 编写逻辑
- style -> 编写样式


![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/32607f6cc15948b2a60d382d5ec05529~tplv-k3u1fbpfcp-watermark.image?)

其中style中的scoped属性，可以让样式成为局部的，不会影响到其他组件，只会作用于当前组件生效。同时在脚手架下支持常见的文件进行模块化操作，例如：图片、样式、.vue文件等。

> 注意：vue cli默认不支持optionsAPI中的templeate写法，只支持template标签写法，要让脚手架支持，需要在`vue.config.json`中配置：

```
module.exports = defineConfig({
  runtimeCompiler: true
})
```


## 脚手架原理之webpack处理html文件和模块打包

[3-8 脚手架原理之webpack处理html文件和模块打包_慕课网 (imooc.com)](https://coding.imooc.com/lesson/602.html#mid=56691)

为了更好的理解项目脚手架的使用，我们来学习一下webpack工具，因为脚手架的底层就是基于webpack工具实现的。

### 安装

webpack工具是基于nodejs的，所以首先要有nodejs环境，其次需要下载两个模块，一个是代码中使用的webpack模块，另一个是终端中使用的webpack-cli模块。

```she
npm install --save-dev webpack
npm install --save-dev webpack-cli
```

先新建目录，使用`npm init -y`初始化package.json，然后安装webpack。

### 配置文件

通过编写webpack.config.js文件，来编写webpack的配置信息，完成工具的操作行为。webpack最大的优点就是可以把模块化的JS文件进行合并打包，这样可以减少网络请求数，具体是通过entry和output这两个字段来完成的。

```javascript
// webpack.config.js 
module.exports = {
  entry: {
    main: './src/main.js'
  },
  output: {
    path: __dirname + '/dist',
    clean: true
  }
}
```

^311103

在终端中进行nodejs脚本build的调用，这样去进行webpack执行，需要我们自己配置一下package.json的脚本。

```shell
"scripts": {
    "build": "webpack"
}
```


```shell
npm run build   # -> webpack
```

这样在项目目录下就产生了一个 /dist 文件夹，里面有合并打包后的文件。那么我们该如何预览这个文件呢？一般可通过html文件进行引入，然后再通过浏览器进行访问。

但是html的编写还需要我们自己引入要预览的JS文件，不是特别的方便，所以是否可以做到自动完成html的操作呢？答案是可以利用webpack工具的插件HtmlWebpackPlugin来实现。

这样HtmlWebpackPlugin插件是需要安装的，通过`npm i HtmlWebpackPlugin`来完成。

```javascript
// webpack.config.js
module.exports = {
    ...,
    plugins: [
        new HtmlWebpackPlugin({
          template: './public/index.html',
          title: 'vue-study'
        }),
        new VueLoaderPlugin()
      ]
}
```


## 脚手架原理之webpack启动服务器和处理sourcemap

### 启动服务环境

目前我们的webpack是没有服务环境的，那么如何启动一个web服务器呢？可以通过webpack-dev-server模块，下载使用即可。

```she
npm install webpack-dev-server
```

安装好后，再package.json中配置scripts脚本，`"serve": "webpack-dev-server"`，然后运行serve脚本。这样就会启动一个http://localhost:8080的服务。

当开启了web服务后，咱们的/dist文件可以不用存在了，服务会把dist的资源打入到内存中，这样可以大大加快编译的速度，所以/dist文件夹可以删除掉了，不影响服务的启动和访问。

### 处理sourcemap

socurcemap启动映射文件的作用，可以通过浏览器查找到原始的文件，这样对于调试是非常有帮助的(否则点击报错信息调整的是编译后的dist文件，而不是src文件)。配置如下：

```js
module.exports = {
    devtool: 'inline-source-map'
}
```


## 脚手架原理之webpack处理样式模块和图片模块

### loader预处理文件

在模块化使用中，默认只会支持JS文件，那么怎么能够让其他类型的文件，例如：css文件、图片文件、json文件等等都可以当作模块化进行使用呢？这就需要使用loader技术。

### 支持css模块化

可以通过安装，`css-loader`和`style-loader`这两个模块来支持css模块化操作。其中`css-loader`作用是让css文件能够import方式进行使用，而`style-loader`的作用是把css代码抽离到`<style>`标签中，这样样式就可以在页面中生效。

```js
module.exports = {
    module: {
    	rules: [
            {
                test: /.css$/i,
                use: ['style-loader', 'css-loader']
            }
        ]
    }
}
```

注意数组的顺序是先执行后面再执行前面，所以先写`style-loader`再写`css-loader`，这样就可以通过`import './assets/common.css'`在main.js中进行使用。

### 图片模块

同样的情况，如果让webpack支持图片模块化也要使用对应的loader，不过在最新版本的webpack中已经内置了对图片的处理，所以只需要配置好信息就可以支持图片模块化。

```js
module.exports = {
    module: {
    	rules: [
            ...,
            {
                test: /.(png|jpg|gif)$/i,
                type: 'asset/resource'
            }
        ]
    }
}
```


## 脚手架原理之webpack处理单文件组件及loader转换

### 处理单文件组件

目前我们的webpack还不支持对.vue文件的识别，也不支持.vue模块化使用，所以需要安装一些模块来实现。

```she
npm install vue @vue/complier-sfc vue-loader
```

vue模块主要是为了让vue功能生效。@vue/complier-sfc是对单文件组件的支持。vue-loader是把单文件组件进行转换。下面看一下webpack的完整配置，如下：

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { VueLoaderPlugin } = require('vue-loader');

module.exports = {
  entry: {
    main: './src/main.js'
  },
  output: {
    path: __dirname + '/dist',
    clean: true
  },
  devtool: 'inline-source-map',
  module: {
    rules: [
      {
        test: /.css$/i,
        use: ['style-loader', 'css-loader']
      },
      {
        test: /.(png|jpg|gif)$/i,
        type: 'asset/resource'
      },
      {
        test: /.vue$/i,
        use: ['vue-loader']
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './public/index.html',
      title: 'vue-study'
    }),
    new VueLoaderPlugin()
  ],
  mode: 'development'
};
```