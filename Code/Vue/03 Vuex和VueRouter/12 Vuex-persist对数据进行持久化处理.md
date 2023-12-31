
默认情况下Vuex状态管理是不会对数据进行持久化存储的，也就是说当我们刷新浏览器后，共享状态就会被还原。那么我们想要在刷新的时候能够保持成修改后的数据就需要进行持久化存储，比较常见的持久化存储方案就是利用本地存储来完成，不过自己去实现还是比较不方便的，下面我们通过第三方模块来实现其功能。

模块github地址：https://github.com/championswimmer/vuex-persist
根据地址要求的配置操作如下：

```js
// 安装：npm install vuex-persist

// @/store/index.js
import VuexPersistence from 'vuex-persist';
const vuexLocal = new VuexPersistence({
  storage: window.localStorage,
  reducer: (state) => ({count: state.count})
})

const store = createStore({
  state: {
    count: 0,
    msg: 'hello'
  }
  plugins: [vuexLocal.plugin]
});
```

默认情况下，vuex-persist会对所有共享状态进行持久化，那么如果我们只需要对指定的属性进行持久化就需要配置 reducer字段，这个字段可以指定需要持久化的状态。

这样当我们修改了state下的count，那么刷新的时候就不会还原了，并且通过chrome浏览器中Application下的Local Storage可以进行查看。
