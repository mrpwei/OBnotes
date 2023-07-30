
## Window.localStorage

只读的`localStorage` 属性允许你访问一个[Document](https://developer.mozilla.org/zh-CN/docs/Web/API/Document) 的对象 [Storage](https://developer.mozilla.org/zh-CN/docs/Web/API/Storage)；存储的数据将保存在浏览器会话中。localStorage 类似 sessionStorage，但其区别在于：存储在 localStorage 的数据可以长期保留；而当页面会话结束——也就是说，当页面被关闭时，存储在 sessionStorage 的数据会被清除。[Window.localStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/localStorage)

## Window.sessionStorage

`sessionStorage` 属性允许你访问一个对应当前Document源的 session [Storage](https://developer.mozilla.org/zh-CN/docs/Web/API/Storage) 对象。它与localStorage 相似，不同之处在于 localStorage 里面存储的数据没有过期时间设置，而存储在 sessionStorage 里面的数据在页面会话结束时会被清除。

-   页面会话在浏览器打开期间一直保持，并且重新加载或恢复页面仍会保持原来的页面会话。
-   **在新标签或窗口打开一个页面时会复制顶级浏览会话的上下文作为新会话的上下文**， 这点和 session cookies 的运行方式不同。
-   打开多个相同的 URL 的 Tabs 页面，会创建各自的 `sessionStorage`。
-   关闭对应浏览器标签或窗口，会清除对应的 `sessionStorage`。

[Window.sessionStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/sessionStorage)

应注意，无论数据存储在 localStorage 还是 sessionStorage，**它们都特定于页面的协议**。也就 是说 `http://example.com` 与 `https://example.com` 的 sessionStorage 相互隔离。

被存储的键值对总是以 UTF-16 [DOMString](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String) 的格式（字符串的形式）来存储，其使用两个字节来表示一个字符。对于对象、整数 key 值会自动转换成字符串形式。


```ad-summary
- localStorage没有过期时间，sessionStorage的数据在页面会话结束时会被清除
```