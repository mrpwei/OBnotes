[File system | Node.js v18.15.0 Documentation (nodejs.org)](https://nodejs.org/dist/latest-v18.x/docs/api/fs.html)

文件树：

```text
.
├── files
│   ├── lorem.txt
│   └── starter.txt
└── index.js
```

1、使用 `fs.readFile` 读取文件，文件读取是异步的

```js
const fs = require("fs");

fs.readFile("./files/starter.txt", (err, data) => {
  if (err) throw err;
  console.log(data.toString());
});

//or

fs.readFile("./files/starter.txt", 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

另外，nodeJS 建议加上 exit on uncaught errors：

```js
const fs = require("fs");

fs.readFile("./files/starter.txt", (err, data) => {
  if (err) throw err;
  console.log(data.toString());
});

// exit on uncaught errors
process.on("uncaughtException", (err) => {
  console.error(`There was an uncaught error: ${err}`);
  process.exit(1);
});
```

任何时候都不建议硬编码文件路径，在不同系统上可能会出现问题

```js
const fs = require("fs");
const path = require("path");

fs.readFile(path.join(__dirname, "files", "starter.txt"), 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data);
});

console.log('Hello'); // 会先输出Hello

// exit on uncaught errors
process.on("uncaughtException", (err) => {
  console.error(`There was an uncaught error: ${err}`);
  process.exit(1);
});
```

2、使用 `fs.writeFile` 写入文件。写入文件不需要指定 utf8，默认以 utf8 格式写入。`fs.appendFile` 用于追加内容。

```js
fs.writeFile(
  path.join(__dirname, "files", "reply.txt"),
  "Nice to meet you",
  (err) => {
    if (err) throw err;
    console.log("Write complete");
  }
);

fs.appendFile(
  path.join(__dirname, "files", "test.txt"),
  "test append",
  (err) => {
    if (err) throw err;
    console.log("Append complete");
  }
);
```

由于这些操作都是异步的，为了保证执行顺序，可以在函数体内嵌套。

```js
fs.writeFile(
  path.join(__dirname, "files", "reply.txt"),
  "Nice to meet you",
  (err) => {
    if (err) throw err;
    console.log("Write complete");

    fs.appendFile(
      path.join(__dirname, "files", "reply.txt"),
      "\n\nYes it is",
      (err) => {
        if (err) throw err;
        console.log("Append complete");

        fs.rename(
          path.join(__dirname, "files", "reply.txt"),
          path.join(__dirname, "files", "newReply.txt"),
          (err) => {
            if (err) throw err;
            console.log("Rename complete");
          }
        );
      }
    );
  }
);
```

这会导致回调地狱，在原生 JS 中我们使用 async/await 避免，在 nodeJS 中也有避免的方法：

```js
const fsPromises = require("fs").promises;
const path = require("path");

const fileOps = async () => {
  try {
    const data = await fsPromises.readFile(
      path.join(__dirname, "files", "starter.txt"),
      "utf8"
    );
    console.log(data);
    await fsPromises.unlink(path.join(__dirname, "files", "starter.txt"));
    await fsPromises.writeFile(
      path.join(__dirname, "files", "promiseWrite.txt"),
      data
    );
    await fsPromises.appendFile(
      path.join(__dirname, "files", "promiseWrite.txt"),
      "Nice to meet you"
    );
    await fsPromises.rename(
      path.join(__dirname, "files", "promiseWrite.txt"),
      path.join(__dirname, "files", "promiseComplete.txt")
    );
    const newData = await fsPromises.readFile(
      path.join(__dirname, "files", "promiseComplete.txt"),
      "utf8"
    );
    console.log(newData);
  } catch (err) {
    console.error(err);
  }
};

fileOps();
```

其中 `unlink` 是删除文件。