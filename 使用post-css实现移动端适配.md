常用移动端适配方案：
- 不同端使用不同代码
- 不同端使用同一套代码，使用 css 样式控制（@media）
- 移动端屏幕适配
	- 利用 rem 按根节点（body）的字体大小来缩放
	- 利用 vh/vw 按屏幕高度和宽度来缩放

rem 适配方案：
- 将 css 属性单位改为 rem
- 动态获取用户设备的屏幕宽度

fontSize (body) = width (屏幕) * fontSize (设计稿中的 body) /width(设计稿中的屏幕)

这样就可以动态适配了。

![](https://imgbed-1305223678.cos.ap-guangzhou.myqcloud.com/202305041537144.png)

1、
```shell
pnpm add postcss autoprefixer postcss-pxtorem -D
```

2、根目录新建 `postcss.config.js`
```js
module.exports = {
  plugins: {
    autoprefixer: {
      overrideBrowerslist: ['Android>=4.0', 'ios>=7'],
    },
    'postcss-pxtorem': {
      //根节点的fontSize
      rootValue: 16,
      propList: ['*'],
      selectorBlackList: [':root'], //vant-ui使用root定义了变量，要忽略
    },
  },
}
```

3、`main.ts`
```ts
...

const app = createApp(App)

app.use(router)

const rootValue = 16
const rootWidth = 390
const deviceWidth = document.documentElement.clientWidth
document.documentElement.style.fontSize = (deviceWidth * rootValue) / rootWidth + 'px'

app.mount('#app')
```

配置好后，post-css 会自动计算 px 转换到 rem 后的值，保证在不同屏幕宽度下是等比例缩放的。