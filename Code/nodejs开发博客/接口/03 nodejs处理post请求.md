
- post请求，即客户端向server端传递数据，如新建博客
- 通过post data传递数据
- 浏览器无法直接模拟，需要手写js，或者使用postman

```js
const http = require("http");

const server = http.createServer((req, res) => {
  if (req.method === "POST") {
    // req数据格式
    console.log("content-type: ", req.headers["content-type"]);
    // 接收数据
    let postData = "";
    req.on("data", (chunk) => {
      postData += chunk.toString();
    });
    req.on("end", () => {
      console.log("postData: ", postData);
      res.end("hello world!");
    });
  }
});
server.listen(8080);
console.log("OK");
```

这里的req.on类似前端的onClick，就是当有数据来的时候（req.on('data')）或者当数据结束传递的时候（req.on('end')）触发。而数据的结束是以数据流的方式进行的，数据流chunk是二进制的，需要toString。

![hf1Our](http://qny.mrpwei.cc/uPic/hf1Our.png)

![7i16YB](http://qny.mrpwei.cc/uPic/7i16YB.png)


nodejs处理路由：
 - https://github.com/
 - https://github.com/username
 - https://github.com/username/xxx

```js
const http = require('http');

const server = http.createServer((req, res) => {
    const url = req.url;
    const path = url.split('?')[0]
    res.end(path); // 返回路由
});
server.listen(8080);
console.log('OK');
```