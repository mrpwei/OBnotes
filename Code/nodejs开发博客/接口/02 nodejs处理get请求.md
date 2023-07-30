nodejs处理http请求：

- get请求和querystring
- post请求和postdata
- 路由

简单示例

```js
const http = require('http');

const server = http.createServer((req, res) => {
    res.end('hello world');
});
server.listen(8080);
```

nodejs处理get请求：

- get请求，即客户端向server端获取数据，如查询博客
- 通过querystring传递数据，如a.html?a=100&b=200
- 浏览器直接访问，就是发送get请求

```js
const http = require('http');
const querystring = require('querystring')

const server = http.createServer((req, res) => {
    console.log(req.method) // GET
    const url = req.url;    // 获取完整url
    req.query = querystring.parse(url.split('?')[1]);
    res.end(JSON.stringify(req.query));
});
server.listen(8080);
console.log('OK');
```

`yarn init`初始化文件夹，配置entry为`app.js`，然后输入上面的代码，在终端输入`node app.js`，然后访问8080端口。