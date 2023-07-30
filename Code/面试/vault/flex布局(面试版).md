#面试

flex布局即弹性布局。设置了`display: flex`的元素就是一个flex container，其所有子元素为容器的成员flex item。

容器默认有两根轴，水平的主轴main axis和垂直的交叉轴cross axis。主轴的开始位置为main start，结束位置为main end；交叉轴为cross start和cross end。

### 容器属性

容器有6个属性：

- `flex-direction`：决定主轴方向

```css
row | row-reverse | column | column-reverse
```

- `flex-wrap`：决定是否换行

```css
nowrap | wrap | wrap-reverse(第一行在下方)
```

- `flex-flow`：是 flex-direction 和 flex-wrap 的简写，默认为 `row wrap`。
- `justify-content`：决定项目在主轴上的对齐方式

```css
flex-start | flex-end | center | space-between | space-around(每个项目两侧的间隔相等)
```

- `align-items`：决定项目在交叉轴上如何对齐

```css
flex-start | flex-end | center | baseline(项目的第一行文字的基线对齐) | stretch(默认值)
```

- `align-content`：定义多根轴线的对齐方式

```css
flex-start | flex-end | center | space-between | space-around | stretch(默认值)
```

### 项目属性

项目也有6个属性：

- `order`：决定项目的顺序，数值越小排列越靠前，默认为0。
- `flex-grow`：定义项目的放大比例，默认为0，即即使存在剩余空间，也不放大。如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。
- `flex-shrink`：定义项目的缩小比例，默认为1，即当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。负值对该属性无效。
- `flex-basis`：定义在分配多余空间之前，项目占据的主轴空间。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。它可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间。
- `flex`：是flex-grow、flex-shrink和flex-basis的简写，默认值为0 1 auto。后两个属性可选。该属性有几个快捷值：auto(1 1 auto)和none(0 0 auto)。建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。[[flex值0、1、none、auto]]
- `align-self`：属性允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`。`auto | flex-start | flex-end | center | baseline | stretch`。

参考：[Flex 布局教程：语法篇 - 阮一峰的网络日志 (ruanyifeng.com)](https://ruanyifeng.com/blog/2015/07/flex-grammar.html)