```bash
fatal: unable to access 'https://github.com/mrpwei/OBnotes.git/': Failed to connect to github.com port 443: Operation timed out
```

在设置中搜索代理，找到代理服务器端口
![image.png](https://imgbed-1305223678.cos.ap-guangzhou.myqcloud.com/20230730141202.png)


```bash
# 注意修改成自己的IP和端口号
git config --global http.proxy http://127.0.0.1:7890 
git config --global https.proxy http://127.0.0.1:7890
```

若按上面改完以后出现这种问题：

```bash
fatal: unable to access 'https://github.com/mrpwei/OBnotes.git/': LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to 127.0.0.1:7890
```

那就改成：

```bash
# 注意修改成自己的IP和端口号
git config --global http.proxy socks5://127.0.0.1:7890 
git config --global https.proxy socks5://127.0.0.1:7890
```