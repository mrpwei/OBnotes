
从本小节我们要完成章节介绍中出现的搜索页面，利用组合式API去实现。先进行开发前的准备工作。

## 数据接口

后端地址：https://github.com/Binaryify/NeteaseCloudMusicApi

后端接口：

​	搜索建议：/search/suggest?keywords=海阔天空

​	搜索结果：/search?keywords=海阔天空

## 反向代理

- 安装axios
- 下载后端接口仓库，启动服务

![VNBHlK](http://qny.mrpwei.cc/uPic/VNBHlK.png)

![3qlEFr](http://qny.mrpwei.cc/uPic/3qlEFr.png)

- 后端端口为3000，前端项目端口为8080，需要解决跨域问题，这里使用反向代理的方式：

可以通过脚手架下的vue.config.js进行反向代理的配置。

```javascript
// vue.config.js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  runtimeCompiler: true,
  devServer: {
    proxy: {
      '/api': {
        target: 'http://localhost:3000',
        changeOrigin: true,   
        pathRewrite: {
          '^/api': ''
        }
      }
    }
  }
})
```

跨域指的是不同的域名，端口，协议中的某一项不同，就不能进行通信。比如前端域名：localhost:8080，后端域名：localhost:3000，这样就不能通信，所以要解决这个问题。

```js
proxy: {
    '/api': {
        target: 'http://localhost:3000'
    }
}
```

表示 `localhost:8080/api/xxxx` 都会去请求 `localhost:3000/api/xxxx`

但是我们的3000下可能没有api这一层，所以通过如下配置：

```js
pathRewrite: {
    '/api': ''    // 3000可以不写api了
} 
```

这样就表示 `localhost:8080/api/xxxx` 都会去请求 `localhost:3000/xxxx`

最后一个配置：

```js
changeOrigin: true
```

表示后端接收到的url是8080而不是3000，这个只是后端可能要用到，一般前端不用管。

可以看看这个视频：https://www.imooc.com/video/22750

配置完成后可以通过`http://localhost:8080/api/search?keywords=海阔天空`进行调用。

![PFYgG6](http://qny.mrpwei.cc/uPic/PFYgG6.png)

## 布局与样式

我们提前把布局和样式做好了，直接在上面进行逻辑的开发，可直接查看 13_search.vue 这个文件，到此我们已经准备好了开发前的准备工作。

## 搜索页面的提示列表功能

```html
<template>
  <div class="search-input">
    <i class="iconfont iconsearch"></i>
    <input type="text" placeholder="搜索歌曲" v-model="searchWord" @input="handleToSuggest">
    <i v-if="searchWord" @click="handleToClose" class="iconfont iconguanbi"></i>
  </div>
  <template v-if="searchType == 1">
    ...
  </template>
  <template v-else-if="searchType == 2">
    ...
  </template>
  <template v-else-if="searchType == 3">
    <div class="search-suggest">
      <div class="search-suggest-head">搜索“ {{ searchWord }} ”</div>
      <div class="search-suggest-item" v-for="item in suggestList" :key="item.id" @click="handleItemResult(item.name), handleAddHistory(item.name)">
        <i class="iconfont iconsearch"></i>{{ item.name }}
      </div>
    </div>
  </template>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';
import '@/assets/iconfont/iconfont.css';

function useSearch(){
  let searchType = ref(1);
  let searchWord = ref('');
  let handleToClose = () => {
    searchWord.value = '';
    searchType.value = 1;
  };
  return {
    searchType,
    searchWord,
    handleToClose
  }
}

function useSuggest(){
  let suggestList = ref([]);
  let handleToSuggest = () => {
    if(searchWord.value){
      searchType.value = 3;
      axios.get(`/api/search/suggest?keywords=${searchWord.value}`).then((res)=>{
        let result = res.data.result;
        if(!result.order){
          return;
        }
        let tmp = [];
        for(let i=0; i<result.order.length; i++){
          tmp.push(...result[result.order[i]]);
        }
        suggestList.value = tmp;
      })
    }
    else{
      searchType.value = 1;
    }
  };
  return {
    suggestList,
    handleToSuggest
  }
}

let { searchType, searchWord, handleToClose } = useSearch();
let { suggestList, handleToSuggest } = useSuggest();

</script>
```

## 搜索页面的结果列表功能

```html
<template>
  <div class="search-input">
    <i class="iconfont iconsearch"></i>
    <input type="text" placeholder="搜索歌曲" v-model="searchWord" @input="handleToSuggest" @keydown.enter="handleToResult($event)">
    <i v-if="searchWord" @click="handleToClose" class="iconfont iconguanbi"></i>
  </div>
  <template v-if="searchType == 1">
    ...
  </template>
  <template v-else-if="searchType == 2">
    <div class="search-result">
      <div class="search-result-item" v-for="item in resultList" :key="item.id">
        <div class="search-result-word">
          <div>{{ item.name }}</div>
        </div>
        <i class="iconfont iconbofang"></i>
      </div>
    </div>
  </template>
  <template v-else-if="searchType == 3">
    ...
  </template>
</template>
<script setup>
import { ref } from 'vue';
import axios from 'axios';
import '@/assets/iconfont/iconfont.css';

function useSearch(){
  ...
}
function useSuggest(){
  ...
}
function useResult(){
  let resultList = ref([]);
  let handleToResult = () => {
    if(!searchWord.value){
      return;
    }
    axios.get(`/api/search?keywords=${searchWord.value}`).then((res)=>{
      let result = res.data.result;
      if(!result.songs){
        return;
      }
      searchType.value = 2;
      resultList.value = result.songs;
    })
  };
  let handleItemResult = (name) => {
    searchWord.value = name;
    handleToResult();
  };
  return {
    resultList,
    handleToResult,
    handleItemResult
  }
}  

let { searchType, searchWord, handleToClose } = useSearch();
let { suggestList, handleToSuggest } = useSuggest();
let { resultList, handleToResult, handleItemResult } = useResult();
</script>
```

## 搜索页面的历史列表功能。

```html
<template>
  <div class="search-input">
    <i class="iconfont iconsearch"></i>
    <input type="text" placeholder="搜索歌曲" v-model="searchWord" @input="handleToSuggest" @keydown.enter="handleToResult($event), handleAddHistory($event)">
    <i v-if="searchWord" @click="handleToClose" class="iconfont iconguanbi"></i>
  </div>
  <template v-if="searchType == 1">
    <div class="search-history">
      <div class="search-history-head">
        <span>历史记录</span>
        <i class="iconfont iconlajitong" @click="handleToClear"></i>
      </div>
      <div class="search-history-list">
        <div v-for="item in historyList" :key="item" @click="handleItemResult(item)">{{ item }}</div>
      </div>
    </div>
  </template>
  <template v-else-if="searchType == 2">
    ...
  </template>
  <template v-else-if="searchType == 3">
    ...
  </template>
</template>
<script setup>
import { ref } from 'vue';
import axios from 'axios';
import '@/assets/iconfont/iconfont.css';

function useSearch(){
  ...
}
function useSuggest(){
  ...
}
function useResult(){
  ...
}  
function useHistory(){
  let key = 'searchHistory';
  let getSearchHistory = () => {
    return JSON.parse(localStorage.getItem(key) || '[]');
  };
  let setSearchHistory = (list) => {
    localStorage.setItem(key, JSON.stringify(list));
  };
  let clearSearchHistory = () => {
    localStorage.removeItem(key);
  };
  let historyList = ref(getSearchHistory());
  let handleAddHistory = (arg) => {
    if(!searchWord.value){
      return;
    }
    if(typeof arg === 'string'){
      searchWord.value = arg;
    }
    historyList.value.unshift(searchWord.value);
    historyList.value = [...new Set(historyList.value)]; //去重
    setSearchHistory(historyList.value);
  };
  let handleToClear = () => {
    clearSearchHistory();
    historyList.value = [];
  };
  return {
    historyList,
    handleAddHistory,
    handleToClear
  };
} 

let { searchType, searchWord, handleToClose } = useSearch();
let { suggestList, handleToSuggest } = useSuggest();
let { resultList, handleToResult, handleItemResult } = useResult();
let { historyList, handleAddHistory, handleToClear } = useHistory();
</script>
```

当一个事件要绑定两个方法时，可以使用加括号的写法：
```html
<input type="text" placeholder="搜索歌曲" v-model="searchWord" @input="handleToSuggest" @keydown.enter="handleToResult">

<input type="text" placeholder="搜索歌曲" v-model="searchWord" @input="handleToSuggest" @keydown.enter="handleToResult($event), handleAddHistory($event)">
```