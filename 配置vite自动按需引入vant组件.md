全局注册组件：
- import 组件
- import 组件样式
- 使用 `app.use` 注册组件

### Tree shaking

- 移除无用的资源，包括 JS 代码、CSS 文件

vant 自带 tree-shaking，但是 css 不支持，可以使用 `unplugin-vue-components` 插件按需引入组件样式：
- 安装插件
- 在 `vite.config.js` 中配置插件
- 直接在模板使用 vant 组件
- 引入函数组件的样式

配置好以后就不用在每个 template 里单独引入组件了，相当于全局注册的方式，同时又不好把不用的代码打包。【视频 4-15】

