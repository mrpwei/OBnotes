什么是 axios？

- 基于 promise 的网络请求库
- 同构：同一套代码可以运行在浏览器和 nodeJS 中
	- 在服务端，使用原生 nodeJS 的 http 模块
	- 在客户端（浏览器），使用 XMLHttpRequests

特性：

- 支持 promise API
- 拦截请求和响应
- 取消请求
- 自动转换 JSON 数据

什么是 http 状态码？

表明特点 http 请求是否成功完成

- 信息响应（100-199）
- 成功响应（200-299）
- 重定向消息（300-399）
- 客户端错误响应（400-499）
- 服务端错误响应（500-599）

什么是请求拦截？

在请求或响应被 then 或 catch 处理前拦截它们

什么是业务状态码？

扩展请求的状态

![](https://imgbed-1305223678.cos.ap-guangzhou.myqcloud.com/202303231957203.png)


`@/api/test.ts`
```ts
import axios from './base'

export const fetchTest = () => {
  return axios.get('test')
}
```

`@/api/bast.ts`
```ts
import axios from 'axios'
import { showDialog } from 'vant'

const instance = axios.create({
  baseURL: '/api',
})

instance.interceptors.response.use((response) => {
  const { data: _data } = response
  const { data, code, msg } = _data
  if (code !== 0) {
    showDialog({
      message: msg,
    }).then(() => {
      //关闭弹窗逻辑
    })
    return Promise.reject(msg)
  }
  return data
})

export default instance

```


