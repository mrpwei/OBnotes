什么是跨域？

同源：

- 协议 Protocol
- 端口 port
- 域名 host

为什么要限制跨域？

- 出于安全性，浏览器会限制脚本内发起跨源 http 请求
	- XMLHttpRequest 和 Fetch API
- Web 应用
	- 只能从加载应用程序的同一个请求 HTTP 资源
	- 除非包含 CORS 响应头

如何解决跨域？

- jsonp：`<script>` 不受同源策略限制
- 跨源域资源共享 CORS：运行 web 应用服务器进行跨源访问控制
- 使不同的源变成同源：反向代理

什么是反向代理？

代理：请求转发

代理服务器类型：

- 正向代理：客户端告诉代理服务器资源的地址，就像打电话
- 反向代理：客户端只告诉要什么资源

![](https://imgbed-1305223678.cos.ap-guangzhou.myqcloud.com/202303231943427.png)

![](https://imgbed-1305223678.cos.ap-guangzhou.myqcloud.com/202303231943019.png)

`vite.confit.ts` 中配置server：

```ts
export default defineConfig({
  plugins: [vue()],
  server: {
	port: 3000,
    proxy: {
      '/api': 'http://localhost:8000',
      '/imgs': 'http://lcalhost:8000',
    },
  },
})
```