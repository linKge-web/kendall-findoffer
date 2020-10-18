## 1.css盒模型
盒子模型是指用来装页面上元素的矩形区域，CSS的盒子模型包括I**E盒子模型**和**标准W3C的模型**。区别在于`width`，IE盒子中`width`表示`Content+padding+border`。

`box-sizing`有三种属性:   
一种是`content-box`，一种是`border-box`,还有一个是从父元素继承的`inherit`。现在还有一个`padding-box`。

- `content-box`一 宽高应用到内容框

- `border-box`— 宽高包括了内边距和边框

- `padding-box`—高宽包括了内边距
  
**应用场景：**

- （1）表单：   
表单中有一些`input`元素其实还是展现的是传统IE盒模型，带有一些默认的样式，而且在不同平台或者浏览器下的表现又不一样，造成了表单展现的差异。此时我们可以通过`box-sizing`属性来构建一个风格统一的表单元素。

- （2）设置子类元素的`margin`或者`border`时，可能会撑破父层元素的尺寸，这时我就需要使用`box-sizing: border-box`来将`border`包含进元素的尺寸中，这样就不会存在撑破父层元素的情况了。

## 2.行内元素有哪些？块级元素有哪些？空元素有哪些?
首先：CSS规范规定，每个元素都有`display`属性，确定该元素的类型，每个元素都有默认的`display`值，如`div`的`display`默认值为`“block”`，则为“块级”元素；`span`默认`display`属性值为`“inline”`，是“行内”元素。

- （1）行内元素有：`a b span img input select strong（强调的语气）`
- （2）块级元素有：`div ul ol li dl dt dd h1 h2 h3 h4… p`
- （3）常见的空元素：
`<br><hr><img><input><link><meta>`    
还有鲜为人知的空元素：
`<area><base><col><command><embed><keygen><param><source><track><wbr>`

### 行内元素和块级元素的区别与互换
#### 区别

- 行内元素会在一行水平方向排列，块级元素**独占一行**，自动填满父级元素
- 块级元素可以包含行内元素和块级元素，行内只能包含文本和其他行内元素
盒模型属性上，行内元素`width` `height`无效，`pading`和`margin`垂直方向上无效

#### 互换

`display：inline/block`

#### inline-block元素

既可以设置高宽，又有inline元素不换行的特性。

### inline-block、inline和block的区别？
- `block`是块级元素，能设置宽高，`margin/padding`都有效前后都有换行符

- `inline`设置宽高无效，`margin`在竖直方向无效，`padding`有效，前后无换行符

- `inline-block`可以设置宽高，`margin/padding`有效，前后无换行符。
  
### 为什么img是inline还可以设置宽高？
`img`是可替换元素，这类元素的展现效果不是由`CSS`来控制的。他们是一种外部对象，外观的渲染独立于`CSS`。内容不受当前文档的样式影响，`CSS`可以影响可替换元素的位置，但是不会影响到可替换元素自身的内容。（比如`iframe`，可能有自己的样式表，不会继承父文档的样式）
可替换元素有内置宽高，性质同设置了`inline-block`一样。

### CSS 中 inline 元素可以设置 padding 和 margin 吗？
- `width`、`height`可以设置，但是没有用。

- `padding` 左右是有用的，上下没有用。

- `margin` 上下左右都有用。

## 3.页面导入样式时，使用 link 和 @import 有什么区别？
- link属于XHTML标签，除了加载`CSS`外，还能用于定义`RSS`, 定义`rel`连接属性等作用；而`@import`是`CSS`提供的，只能用于加载`CSS`;
- 页面被加载的时，`link`会同时被加载，而`@import`引用的`CSS`会等到页面被加载完再加载;
- `@import`是`CSS2.1` 提出的，只在 IE5 以上才能被识别，而`link`是`XHTML`标签，没有兼容问题;
- `link`支持使用`Javascript`控制DOM去改变样式；而`@import`不支持。

## 4.清楚浮动有哪些方式
- （1）在浮动元素下方添加一个非浮动元素
```html
<div>
    <div style="float:left;width:45%;"></div>
    <div style="float:right;width:45%;"></div>
    <div style="clear:both;"></div>
</div>
```
父容器现在必须考虑非浮动子元素的位置，而后者肯定出现在浮动元素下方，所以显示出来，父容器就把所有子元素都包括进去了。这种方法比较简单，但是要在页面中增加冗余标签，违背了语义网的原则。

- (2）将父容器也改成浮动定位
```html
<div style="float:left;">
    <div style="float:left;width:45%;"></div>
    <div style="float:right;width:45%;"></div>
</div>
```
这种方法不用修改HTML代码，但是缺点在于父容器变成浮动以后，会影响到后面元素的定位，而且有时候，父容器是定位死的，无法变成浮动。

- （3）父容器设置overflow: hidden或者auto。（开启BFC）
```html
<div style="overflow: hidden;">
    <div style="float:left;width:45%;"></div>
    <div style="float:right;width:45%;"></div>
</div>
```
给父容器加上`overflow:hidden`，加上之后，形成`BFC`，需要计算超出的大小来隐藏，所以父容器会撑开放入子元素，同时计算浮动的子元素。

**缺点：**一个是IE6不支持，另一个是一旦子元素的大小超过父容器的大小，就会出显示问题。

- （4）利用:after伪选择符，在父容器的尾部自动创建一个子元素 ==（推荐这种）==
```css
.clearfix:after {
    content: "";
    display: block;
    clear: both;
}
/*兼容ie6：激活父元素的"hasLayout"属性，让父元素拥有自己的布局*/
.clearfix {
    zoom: 1;  /*或者height：1%；*/
}
```

## 5.简单介绍BFC和IFC

### BFC —— 块级格式化上下文

- **BFC触发条件：**

- 根元素或其他包含他的元素
- 浮动元素 float：left/right
- position：absolute/fixed
- display：inline-block,table-cell,table-caption
- overflow不为visible
- 弹性盒子：display: flex 或 inline-flex

- **规则：**垂直方向一个一个放；距离由margin决定，同一个bfc里面相邻会重叠；不会和浮动元素重叠；计算高度时浮动子元素也计算；容器内与容器外互不影响。

- **主要用途：**清除浮动 防止margin重叠

### IFC —— 行内格式化上下文

- （1）创建方式：

和BFC相比，它的创建方式是被动的、隐式的，是由所包含的子元素来创建：只有在一个区域内仅包含可水平排列的元素时才会生成，这些子元素可以是`文本`、`inline-level元素`或`inline-block-level元素`。

- （2）特性：

IFC内部的元素，按从左到右、从上到下的顺序排布；
IFC内部的每个元素，都可以通过设置`vertical-align`属性，来调整在垂直方向上的对齐；
包含这些内部元素的矩形区域，形成的每一行，被称为`line box`（行框）。

## 6.选择器优先级

> ！important>行内样式>id选择器>类选择器>标签选择器>通配符>继承

权重算法：`(0,0,0,0)` ——> 第一个0对应的是`important`的个数，第二个0对应的是`id`选择器的个数，第三个0对应的`类`选择器的个数，第四个0对应的是`标签`选择器的个数，合起来就是当前选择器的权重。

比较：先从第一个0开始比较，如果第一个0大，那么说明这个选择器的权重高，如果第一个相同，比较第二个，依次类推。