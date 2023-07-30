[Vite | 下一代的前端工具链 (vitejs.dev)](https://cn.vitejs.dev/)

主要由两部分组成：

- 一个开发服务器：它基于原生 ES 模块，提供了丰富的内建功能，比如速度飞快的模块热更新 (HMR)
- 一套构建指令：它使用 Rollup 打包代码，并且是预配置的，可输出用于生产环境的高度优化过的静态资源

## 为什么选 Vite？

[为什么选 Vite | Vite 官方中文文档 (vitejs.dev)](https://cn.vitejs.dev/guide/why.html)

在浏览器支持 ES 模块之前，JavaScript 并没有提供原生机制让开发者以模块化的方式进行开发，需要使用打包工具，比如 webpack 来构建项目。

在本地开发时，构建工具会打包项目代码，同时启动一个本地开发服务器来运行前端项目。

![](https://imgbed-1305223678.cos.ap-guangzhou.myqcloud.com/202303211508628.png)

当应用越来越大的时候，就会开始出现一些性能问题：

- 启动本地开发服务器的时间会很长
- 热更新也会很慢

Vite 针对上面 2 个问题进行了优化。

- **服务器启动**

Vite 从依赖打包和源码打包 2 个方面提升性能

- 依赖：使用 esbuild 进行依赖预打包，esbuild 使用 go 编写，比 JavaScript-based 的打包工具快 10-100 倍
- 源码：使用浏览器原生 es moudle 提供源码，让浏览器接管打包工具的部分工作

- **热更新**

Vite 在文件热更新上做了优化

- 使用 ESM 不需要重新编译：一些打包工具的开发服务器在文件更改时，需要重新构建整个项目来获取新的模块依赖关系
- 使用浏览器缓存加速：Vite 利用 http 头来加速整个页面的重新加载

![](https://imgbed-1305223678.cos.ap-guangzhou.myqcloud.com/202303211517741.png)

