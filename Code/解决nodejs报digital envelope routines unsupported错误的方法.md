修改package.json

```json
"scripts": {
    "serve": "set NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service serve",
    "build": "set NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service build"
  },
```

或者在终端：
windows:
```shell
set NODE_OPTIONS=--openssl-legacy-provider
```
mac and linux:
```bash
export NODE_OPTIONS=--openssl-legacy-provider
```

接着重新npm run serve即可。