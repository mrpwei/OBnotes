nodejs 和元素 js 有什么不同？

1) Node 运行在服务器，而不是浏览器（是后端而不是前端）
2) 控制台是 terminal window
3) 全局对象是 global 而不是 window
4) 具有一些核心模块（os、fs、path 等）
5) 使用 CommonJS 而不是 ES6 modules
6) 不具备一些 JS APIs，比如 fetch

新建 `start.js`，然后运行 `node start`：

```js
const os = require("os");

console.log(os.type());     // Windows_NT
console.log(os.version());  // Windows 10 Pro
console.log(os.homedir());  // C:\Users\pengwei
```

```js
const path = require("path");

console.log(__dirname);
console.log(__filename);

console.log(path.dirname(__filename));
console.log(path.basename(__filename));
console.log(path.extname(__filename));

console.log(path.parse(__filename));
```

输出：

```text
/Users/pengwei/Desktop/code/node-tutorial
/Users/pengwei/Desktop/code/node-tutorial/start.js
/Users/pengwei/Desktop/code/node-tutorial
start.js
.js
{
  root: '/',
  dir: '/Users/pengwei/Desktop/code/node-tutorial',
  base: 'start.js',
  ext: '.js',
  name: 'start'
}
```

CommonJS 使用 require 和 exports，而不是 import 和 export。

新建 `math.js`

```js
const add = (a, b) => a + b;
const subtract = (a, b) => a - b;
const multiply = (a, b) => a * b;
const divide = (a, b) => a / b;

module.exports = { add, subtract, multiply, divide };
```

`start.js` 中：

```js
const { add, subtract, multiply, divide } = require("./math");
console.log(add(2, 3));

//or
const math = require("./math");
console.log(math.add(2, 3));
```

当然，`math.js` 也可以写成：

```js
exports.add = (a, b) => a + b;
exports.subtract = (a, b) => a - b;
exports.multiply = (a, b) => a * b;
exports.divide = (a, b) => a / b;
```