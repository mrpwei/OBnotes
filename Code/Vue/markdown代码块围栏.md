添加标题栏：

```css
body:not(.no-codeblock-line-numbers)
  .markdown-reading-view
  pre[class*="language-"]::before {
  display: block;
  content: " ";
  line-height: 1.6em;
  background-color: var(--code-block-alt-bg); /* #35393e */
  border-top-right-radius: calc(var(--codeblock-roundness) * 0.8);
  border-top-left-radius: calc(var(--codeblock-roundness) * 0.8);
}
```

效果：

![djgOGR](http://qny.mrpwei.cc/uPic/djgOGR.png)

添加代码类型：

```css
pre[class~="language-javascript"]:before,
pre[class~="language-js"]:before {
    content: "js";
    background: #f7df1e;
    color: var(--theme-ui-colors-black);
}

pre[class*="language-"]:before {
    background-color: var(--theme-ui-colors-white);
    border-radius: 0 0 4px 4px;
    color: var(--theme-ui-colors-black);
    font-size: 12px;
    letter-spacing: 0.035rem;
    padding: 0.1rem 0.5rem;
    position: absolute;
    left: 1rem;
    text-align: right;
    text-transform: uppercase;
    font-weight: 600;
}
```

效果：[Code Block Examples | Minimal Blog (lekoarts.de)](https://minimal-blog.lekoarts.de/code-block-examples)

![8REvWc](http://qny.mrpwei.cc/uPic/8REvWc.png)
