#### HTML篇

##### 1.如何理解HTML语义化？

语义化的含义就是用正确的标签做正确的事情，让页面的内容结构化。

- 在没有样式CSS情况下，也能以一种文档格式显示，并且是容易阅读的
- 有利于SEO，搜索引擎的爬虫依赖于标签来确定上下文以及各关键字的权重
- 内容结构清晰，便于阅读代码和后期的维护理解

##### 2.块级元素，内联元素？

- 块级元素：不管内容多少都独占一行

  div，h1-h5，ul，li，p


- 内联元素：其实又可以分为文本元素和行内块元素，前者不可以设置宽高，只能通过内容撑起宽高，后者可以设置宽高

  span，img，input，button

##### 3.HTML5新增语义化标签

article，aside，footer，header，nav

#### CSS篇

##### 1.盒模型

设置方式：`box-sizing: content-box | border-box`

- 标准盒（content-box，默认）：

  元素所占的宽度：

  `Width = width + padding-left + padding-right + border-left + border-right;`

  元素所占高度：

  `Height = height + padding-top + padding-bottom + border-top + border-bottom;`


- 怪异盒，也叫IE盒模型（border-box）:

  元素所占的宽度：

  `Width = width;`

  `Width = width;`

##### 2.BFC的应用

（1）什么是BFC？

`Block Format Context`，块级格式化上下文，BFC是一个完全独立的渲染空间，内部元素的渲染不会影响到边界以外的元素。

（2）BFC的应用

触发条件：

- `overflow不为visible`
- `float不是none`
- `display为inline-block，flex`
- `position为fixed或者absolute`

（3）解决的问题：

- float的高度塌陷问题
- margin垂直边距重叠问题

##### 3.float布局

引入float布局的初衷是为了实现文字环绕图片的效果，使用float对元素进行定位存在很多问题，因此后来提出的flex布局，很好地解决了这个问题。

清除浮动：

```javascript
.clearfix:after {
    content: '';
    display: block;
    clear: both;
};
```

##### 4.flex布局

（1）基本概念

任何一个容器都可以指定为Flex布局

```javascript
.box{
    display: flex;
}
```

行内元素也可以使用Flex布局

```javascript
.box{
    display: inline-flex;
}
```

> 在设置为Flex布局后，子元素的float、clear和vertical-align属性将失效

采用Flex布局的元素，称为Flex容器，它的成员自动成为容器成员，称为Flex项目

容器默认存在两根轴，水平的主轴和垂直的交叉轴。

（2）容器的属性

- flex-direction：决定主轴的方向，默认为row（项目从左至右依次排列）

  ```javascript
  .box{
      flex-direction: row | row-reverse | column | column-reverse;
  }
  ```

- flex-wrap：决定项目是否换行，默认为no-wrap

  ```javascript
  .box{
      flex-wrap: nowrap | wrap | wrap-reverse(第一行在下方，即进行换行的项目排列在第一行);
  }
  ```

- flex-flow：flex-direction和flex-wrap的简写形式

  ```javascript
  .box{
      flex-flow: <flex-direction> | <flex-wrap>;
  }
  ```

- justify-content：决定项目在主轴上的对齐方式，默认为flex-start

  ```javascript
  .box{
      justify-content: flex-start | flex-end | center | 
          space-between（两端对齐，项目之间的间隔都相等） | 
          space-around(项目两侧的间隔都相等，所以，项目之间的间隔比项目与边框的间隔大一倍);
  }
  ```

- align-items：决定项目在交叉轴上如何对齐，默认为stretch

  ```javascript
  .box{
      align-items: flex-start | flex-end | center | baseline(项目的第一行文字的基线对齐) | stretch(如果项目未设置高度或设为auto，将占满整个容器的高度);
  }
  ```

（3）项目的属性

- order：定义项目的排列顺序，数值越小，排列越靠前，默认为0
- flex-grow：定义项目的放大比例，默认为0，即如果存在剩余空间，要不放大
- flex-shrink：定义项目的缩小比例，默认为1，即如果空间不足，将项目缩小
- flex-basis：定义了在分配多余空间之前，项目占据的主轴空间，浏览器根据这个属性，计算主轴是否有多余空间，默认值为auto，即项目的本身大小
- flex：是flex-grow、flex-shrink和flex-basis的简写，默认值为0 1 auto；auto（1 1 auto）和none（0 0 auto）；
- align-self：允许单个项目与其它项目不一样的对齐方式，可覆盖align-items属性，默认为auto，即继承父元素的align-items属性。

##### 5.伪类和伪元素

（1）伪类：

伪类是选择器的一种，用于选择处于特殊状态的元素，伪类就是开头为冒号的关键字。

下面举个例子：

```html
<div>
	<p></p>
	<p></p>
	<p></p>
</div>
```

当我们要选中第一个p标签时，我们可以使用伪类：

```css
div p:first-child {
    font-size: 20px;
};
```

还有一些伪类只会在用户以某种方式和文档交互的时候应用，这些叫做用户行为伪类，也叫做动态伪类。

例如：`:hover`，`:focus`这些都是只有当用户与元素进行交互的时候才会激活。

（2）伪元素

伪元素表现得就像你往标记文本加入了一个全新得HTML元素一样，开头为双冒号：：

常见的就是`::before`，`::after`，`::first-letter`

##### 6.居中对齐方式

（1）水平居中

- 文本元素：text-align：center

- 块级元素： 

  margin：0 auto

  子绝父相，left：50% + margin-left：负自身宽度的一半；

  子绝父相，left：50% + transform：translateX（-50%）；


- flex布局：父元素：display：flex，justify-content：center；

（2）垂直居中：

- 文本元素：line-height等于包裹该文本元素的高度

- 块级元素：

  子绝父相，top：50% + margin-top：负自身高度的一半；

  子绝父相，top：50% + transform：translateY（-50%）；


- flex布局：父元素：display：flex，align-items：center；