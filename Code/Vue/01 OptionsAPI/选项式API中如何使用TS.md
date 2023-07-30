
![UAAqvt](http://qny.mrpwei.cc/uPic/UAAqvt.png)

首先全新安装一下脚手架，在定义安装中，选择`TypeScript`选项。

## 选项式API使用TS

在选项式API中可以引入，`defineComponent`方法，这个方法可以对选项式API进行自动类型推断。并且需要在`<script>`标签上明确指定`lang="ts"`这个属性。

```vue
<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
  data(){
    return {
      count: 0
    }
  },
  mounted(){
    this.count = 2   // ✔ 
  }
});
</script>
```

若指定了`lang="ts"`而不用`defineComponent`，则无法推断类型，即使赋值数字给`this.count`也会报错。

在选项式API中可以利用类型断言的方式给响应式数据进行类型注解。

```vue
<script lang="ts">
import { defineComponent } from 'vue'
type Count = number | string;
interface List {
  username: string
  age: number
}
export default defineComponent({
  data(){
    return {
      count: 0 as Count,
      list: [] as List[]
    }
  }
});
</script>
```

像计算属性、方法等功能就可以正常配合TS的类型系统进行使用就好，如果是多个类型可以通过类型保护的方式进行控制，代码如下：

```vue
<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
  ...
  computed: {
    doubleCount(): number|string{
      if(typeof this.count === 'number'){
        return this.count * 2;
      }
      else{
        return this.count;
      }
    }
  },
  methods: {
    handleClick(n: number){
      if(typeof this.count === 'number'){
        this.count += n;
      }
    }
  }
});
</script>
```

