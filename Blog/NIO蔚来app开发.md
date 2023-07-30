使用@别名不认识，需要进行配置。

先在 vite.config.ts 中配置，这时会提示找不到类型，按提示安装 `types/node` 即可。

```ts
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";
import path from "path"; // +

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  resolve: { // +
    alias: { // +
      "@": path.resolve(__dirname, "src"), // +
    },
  },
});
```

然后在 `tsconfig.json` 配置 baseUrl 和 paths：

```json
{
  "compilerOptions": {
    "target": "ESNext",
    "useDefineForClassFields": true,
    "module": "ESNext",
    "moduleResolution": "Node",
    "strict": true,
    "jsx": "preserve",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "esModuleInterop": true,
    "lib": ["ESNext", "DOM"],
    "skipLibCheck": true,
    "noEmit": true,
    "baseUrl": "./",
    "paths": {
      "@": ["./src"],
      "@/*": ["./src/*"]
    }
  },
  "include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

设置字体小于12px：[设置浏览器显示小于12px以下字体的三种方法 - 漫思 - 博客园 (cnblogs.com)](https://www.cnblogs.com/sexintercourse/p/17015571.html#:~:text=%E8%B0%B7%E6%AD%8C%E6%B5%8F%E8%A7%88%E5%99%A8%E4%B8%8A%E6%98%BE%E7%A4%BA,%E6%83%8A%E5%96%9C%EF%BC%8C%E6%84%8F%E4%B8%8D%E6%84%8F%E5%A4%96%EF%BC%9F)

```css
.box2 {
    font-size: 10px;
    transform: scale(0.83333);
    /* transform-origin: 0 0; */
    white-space: nowrap;
}
```

针对 chrome 浏览器，加 webkit 前缀，用 `transform:scale()` 这个属性进行放缩。

Typescript下清除定时器：

```ts
let timer: NodeJS.Timer | null = null;
const autoPlay = () => {
  timer = setInterval(() => {
    nextSlide();
  }, 2000);
};

onMounted(() => {
  autoPlay();
});

onBeforeUnmount(() => {
  // 清理定时器要转换timer的类型
  clearInterval(Number(timer));
  timer = null;
});
```

beforeDestroy不生效？：[在 Vue 里如何优雅的清除一个定时器？ - 掘金 (juejin.cn)](https://juejin.cn/post/6987227498912153607)

[CSS3 图片裁剪居中 object-fitt: cover、fill、contain 属性详解 - 简明教程 (jmjc.tech)](https://www.jmjc.tech/less/166)
[CSS3 背景 - 简明教程 (jmjc.tech)](https://www.jmjc.tech/less/27)