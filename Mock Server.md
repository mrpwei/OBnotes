使用 json-server 搭建 mock server

使用 json-server 的 2 种方式：

- 通过 json-server 命令启动一个服务
- 通过 module 将 json-server 引入到自己的 node 服务

### ele-h5-server 架构

![](https://imgbed-1305223678.cos.ap-guangzhou.myqcloud.com/202303231839818.png)

- middleware：中间件，用来处理所有请求，比如鉴权、静态资源等功能
	- json-server 提供的中间件：静态资源、请求体解析
	- 自定义中间件：鉴权——service
- router：带路由 url 的中间件，处理特定路由 url 的请求
	- json-server 的路由：一些直接使用 json 数据的 api
	- 自定义路由：有自定义逻辑的 api——controller

### 文件结构

建立项目文件夹，pnpm 初始化，然后新建一下文件结构：

```text
.
├── data
├── package.json
├── public
└── src
    ├── app.js
    ├── controller
    ├── db.js
    ├── router.js
    └── service
```

- `data`：存放所有数据 json 文件
- `public`：存放静态资源
- `src`：项目的处理逻辑
	- `app.js`：项目入口文件，包括应用创建、中间件使用
	- `router.js`：处理自定义路由
	- `db.js`：处理 json-server 的路由
	- `controller`：存放 controller
	- `service`：存放 service

nodemon 可以自动重启服务器，开发环境安装后再 package. json 配置：

```
"scripts": {
    "server": "nodemon --delay 1000ms ./src/app.js"
  },
```