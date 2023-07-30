有三个文件，分别是a.json、b.json、c.json：

```js
// a.json
{
	next: 'b.json',
	msg: 'this is a'
}

// b.json
{
	next: 'c.json',
	msg: 'this is b'
}

// c.json
{
	next: 'null',
	msg: 'this is c'
}
```

需求是首先读取 a 的内容，然后根据 next 读取 b 的内容，最后读取 c 文件内容。

```js
const fs = require('fs')
const path = require('path')

const fullPathName = path.resolve(__dirname, 'a.json')
fs.readFile(fullFileName, (err, data) => {
	if(err) {
		console.error(err)
		return
	}
	console.log(JSON.parse(data.toString()))
})
```

普通回调函数方式：

```js
function getFileContent(fileName, callback) {
	const fullPathName = path.resolve(__dirname, fileName)
	fs.readFile(fullFileName, (err, data) => {
		if(err) {
			console.error(err)
			return
		}
		callback(JSON.parse(data.toString()))
	})
}

getFileContent('a.json', aData => {
	console.log('a data', aData)
	getFileContent(aData.next, bData => {
		console.log('b data', bData)
		getFileContent(bData.next, cData => {
			console.log('c data', cData)
		})
	})
})
```

可以看到会一直嵌套，造成 callback hell。

Promise 方式：

```js
function getFileContent(fileName) {
	const promise = new Promise((resolve, reject) => {
		const fullPathName = path.resolve(__dirname, 'a.json')
		fs.readFile(fullFileName, (err, data) => {
			if(err) {
				console.error(err)
				return
			}
			resolve(JSON.parse(data.toString()))
		})
	})
	return promise
}

getFileContent('a.json').then(aData => {
	console.log('a data', aData)
	return getFileContent(aData.next)
}).then(bData => {
	console.log('b data', bData)
	return getFileContent(bData.next)
}).then(cData => {
	console.log('c data', cData)
})
```

