---
title: css笔记
date: 2018-07-19 15:26:11
tags:
- 前端
- CSS
categories: 编程
---

## CSS是用来干什么的？
CSS全名叫Cascading Style Sheets，中文翻译为“层叠样式表”，简称样式表，它是一种制作网页的新技术。 
HTML文件就只包括资料，而CSS则是告诉浏览器这些资料应该要如何显现出来。
打个比方，HTML是房子的框架，而CSS就是室内室外的装潢。css让网页变得好看。

## 如何在HTML中引入？

（1）外联式(推荐)

通过`<link>`标记来引入外部的CSS文件(.css)。

可以被其它网页共享。

格式：`<link href=“CSS的URL” rel=“stylesheet” type=“text/css” />`

注意：`<link>`标记只能放在`<head>`中

（2）内嵌式

通过`<style></style>`来书写CSS代码。

只能应用于当前网页，不能被其它网页共享。

注意：`<style>`标记可以放在网页的任何地方，但一般放在`<head>`。

（3）行内样式

通过style的属性来书写CSS代码。

每一个HTML元素，都有 style、class、id、name、title 属性。

举例：`<p style=“font-size:24px;”></p>`

**关于外联式路径(url)补充：**

绝对路径：从是指目录下的*绝对*位置，直接到达目标位置，通常是从盘符开始的*路径*。 

比如：D:\前端学习\H5+CSS3视频\CSS3

相对路径（推荐）：由这个文件所在的*路径*引起的跟其它文件（或文件夹）的*路径*关系。 

比如一个目录结构：

```
文件夹/
├── index.html/
├── img/
│   ├── img1.img/
│   ├── img2.img/
└── css/
    └── main.css/
```

你的html文档是index.html，你想要引入main.css,路径为：css/main.css    (`/`相当于进入)

你的CSS文件时main.css，你想要引入一个背景图片img1.img(后面会有专门的内容介绍背景的知识)

路径为：../img/img1.img   (`..` 相当于上一级)

## 基础语法

CSS 规则由两个主要的部分构成：选择器，以及一条或多条声明。

```
选择器 {声明1; 声明2; 声明3 }
```

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/lwpBJD3in0sRl6dCsHyA*YuxhRfQ4otNNzJgTCD0f4U!/b/dC8BAAAAAAAA&bo=XQF3AF0BdwABCS4!&rf=viewer_4) 

## 选择器

选择器就是选中html中的元素，方便对其的样式进行修改

**比较常用的有 类选择器、ID选择器、结构（位置）伪类选择器、并集选择器、后代选择器，子元素选择器**

### 标签选择器

标签选择器是指用HTML标签名作为选择器的，按标签名进行分类。

```css
标签名 ｛样式;}
```

标签选择器最大的优点是快速为页面中同类型的标签统一样式，但是不能进行差异化设计。

### 类选择器

使用 `.类名`  表示

```css
.类名｛样式;｝
```

标签调用是通过 class="类名" 来调用

#### 多类名选择器

 我们可以给标签指定多个类名，从而达到给多选择的目的。

```css
<p class="类名1 类名2"></p>
```

```txt
1. 样式的显示效果与HTML中的类名先后顺序无关，受CSS样式的书写上下顺序影响
2. 各个类名中间用空格隔开
```

### ID选择器

id选择器使用 # 进行标识 ，后面紧跟id名

```css
#id名｛样式;}
```

### 通配符选择器

用 * 号进行标识。他是所有选择器中作用范围最广的，能匹配页面中的所有元素

通常用来清除元素边距（不推荐，有专门的Normalize.css可以用来统一跨浏览器样式）

```css
*{
  margin: 0px;
  padding: 0px;
}
```

### 伪类选择器

伪类选择器通常用于向某些选择器添加特殊效果。

`选择器 伪类选择器`

#### 链接伪类选择器

-  :link  未访问的链接
-  :visited 以访问的链接
- : hover 鼠标移动到链接上
-  : active 选定链接

通常与a标签选择器匹配使用

全部书写的时候顺序不要颠倒。可以单独书写。

#### 结构（位置）伪类选择器（CSS3)

-  :first-child  选取属于器父元素的首个子元素
-  :last-child   选取其父元素的最后一个子元素
- :nth-child(n)  匹配属于其父元素的第N个子元素，n值可以为 `even 偶数` `odd 奇数` n从0开始开始，因此可以是公式，比如3n选中3的倍数，也可以是确定的数字。
- :nth-last-chilf(n)  从最后一个子元素开始记数，匹配第N个元素

选择中的子元素是考虑元素类型的， ”0选取其父元素“相当于选择自己的兄弟元素

##### 目标伪类选择器

:target 选择器用于选取当前活动的目标元素

一般与锚点定位结合使用

### CSS复合选择器

复合选择器是由两个或多个基础选择器，通过不同的组合方式而成的。

#### 交集选择器

交集选择器由两个选择器构成，第一个为标签选择器，第二个为类选择器，两个选择器中间不能有空格。

```css
h1.class{样式;}
```

#### 并集选择器

多个选择器中间用逗号分开（一般换行），所有选择器都会执行后面的样式。

```css
p,
class,
id{样式;}
```

### 后代选择器

后代选择器又称包含选择器，用来选择元素或元素组的后代。

写法：把外层标签写在前面，内层标签写在后面，中间用空格分隔

```css
class p{样式;}
```

### 子元素选择器

子元素选择器只能选择某父元素的子元素，其写法就是把父级标签，子级标签写在后面，中间用 > 连接，符号左后各留有一个空格

```css
.class > p {样式;}
```

子元素选择器只能选择其亲儿子。不包括孙子等。

### 属性选择器

| 选择器         | 含义                   |
| ----------- | -------------------- |
| E[xxx]      | 存在xxx属性              |
| E[xxx=val]  | 属性值完全等于val           |
| E[xxx*=val] | 属性值中包含val字符，并可以在任何位置 |
| E[xxx^=val] | 属性值中包含val字符，并在开始的位置  |
| E[xxx$=val] | 属性值中包含val字符，并在结束的位置  |

E为选择器 xxx为属性值

可以选中拥有该含义的内容

### 伪元素选择器

之所以被称为伪元素，是因为他们不是真正的页面元素，HTML中没有对应的元素，但是其所有用法和表现行为与真正的页面元素一样，可以对其使用诸如页面元素一样的CSS样式，表面上看上去貌似是页面的某些元素展现，实际上是CSS样式展现的行为，因此被称为伪元素

E为选择器

- E::first-letter 文本的第一个单词或字
- E::first-line 文本中的第一行（受页面大小的影响）
- E::selection 改变选中文本时的样式

E::before  与 E::after

必须有content属性

在E元素内部的开始或结束位置创建一个元素，该元素为行内元素。并且不占位置

**E:after与E:before 在旧版本里是伪元素，CSS3中":"用来表示伪类，"::"用来表示伪元素，但是在高版本下E:after与E:before 会被自动识别为E::after 与 E::before**

## 布局

### 盒子模型

盒子模型是html+css中最核心的基础知识，理解了这个重要的概念才能更好的排版，进行页面布局。 

要把整个页面中的元素理解成一个一个盒子（div）

#### 基础概念

CSS盒子模型 又称框模型 (Box Model) ，包含了元素内容（content）、内边距（padding）、边框（border）、外边距（margin）几个要素。如图： 

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/UdzhrAXR2O8lVf0v44AGeBOompLIR9scd.pkcFE4CJU!/b/dFkAAAAAAAAA&bo=9AH0AfQB9AEBCS4!&rf=viewer_4) 

盒子模型顾名思义，你可以将其理解成一个盒子，element的width和height就是物品的大小，padding就是物品距边框的距离，border就是边框的大小，margin就是这个盒子与另外一个盒子的距离

#### 盒子外边距（margin）

margin用于设计外边距，设置外边距会在元素之间创建空白。

##### 外边距实现盒子居中

可以让一个盒子实现水平居中，需要满足：
1必须是块级元素

2盒子必须指定了宽度

然后给盒子左右外边距设置 auto 就可以使盒子居中

`margin:0 auto;`

**注意：**行内元素只有左右外边距，没有上下外边距

##### 外边距的合并

###### 相邻块元素垂直外边距的合并

当上下相邻的两个块元素相遇时，如果上面的元素有下外边距margin-bottom，下面的元素有上外边距margin-top，则他们之间的垂直间距不是margin-bottom与margin-top之和，而是两者中的较大者。这种现象称为相邻块状元素外边距的合并（也称为外边距塌陷）。

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/ygnB*F*blcn1u2mvGgyfjgRgE7kXqv0V9XQ3AmpVQg8!/b/dPMAAAAAAAAA&bo=uwKgAQAAAAARByg!&rf=viewer_4) 

###### 嵌套块元素垂直外边距的合并

对两个嵌套关系的块元素，如果父元素没有上内边距级边框，则父元素的上外边距会与子元素的上外边距发生合并，合并后外边距为两者的较大者，即使父元素的上外边距为0，也会发生合并。

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/DDDeXiMkwJ*SNxR1cM0LSYEZJQDP8qm.OE02VDW85zo!/b/dGcBAAAAAAAA&bo=eAIIAQAAAAARF1M!&rf=viewer_4) 

解决方案：
1 可以为父元素定义1像素的上边框或上内边距，

2 可以为父元素添加overflow:hidden。

#### 盒子边框（border)

语法：

`border：border-width||border-style||border-color`

##### 边框样式

要先有边框的宽度和颜色

```txt
none：没有边框样式 （默认值）
solid: 边框为单实线
dashed: 边框为虚线
dotted: 边框为点线
double: 边框为双实线
```

可以单独为盒子的一个边框设置样式

##### 表格的细线边框

`border-collapse:collapse`  合并相邻的边框

##### 圆角边框（CSS3）

`border-radius: 左上角 右上角 右下角 左下角；`

| 值       | 描述                     | 测试                                                         |
| -------- | ------------------------ | ------------------------------------------------------------ |
| *length* | 定义圆角的形状。         | [测试](http://www.w3school.com.cn/tiy/c.asp?f=css_border-radius) |
| *%*      | 以百分比定义圆角的形状。 | [测试](http://www.w3school.com.cn/tiy/c.asp?f=css_border-radius&p=6) |

#### 盒子内边距（padding）

padding用于设计内边距，指边框与内容之间的距离

| 值的个数 | 表达意思                                        |
| -------- | ----------------------------------------------- |
| 1个值    | 上下左右的边距                                  |
| 2个值    | padding:3px 5px; 上下3像素 左右5像素            |
| 3个值    | padding:3px 5px 10px;上3像素 左右5像素 下10像素 |
| 4个值    | padding:上 右 下 左                             |

也可以单独为某一方向设置边距

#### content宽度和高度

使用宽度属性width和高度属性可以对盒子的大小进行控制。

**注意：**

1 宽度属性width和高度属性height仅使用与块级元素，对行内元素无效（img和input除外）

2 计算盒子总高度时要考虑上下两个盒子的垂直外边距合并的情况

3 如果一个盒子**没有给定宽度或者高度或者**继承父亲的宽度和高度，则padding不会影响盒子的大小

**如果已经给一个盒子指定了宽高，再使用padding会把盒子撑开，定义盒子内容宽高时因考虑这个问题**

#### 盒子的大小

##### box-sizing (CSS3)

用来修正给盒子增加padding和boder后改变了盒子的大小的问题

**content-box** 

默认值，宽度和高度分别应用到元素的内容框。在宽度和高度之外绘制元素的内边距和边框。

也就是说padding和margin会在content外添加。**盒子大小=widith+padding+border**

**border-box**

为元素设定的宽度和高度决定了元素的边框盒。

就是说，为元素指定的任何内边距和边框都将在已设定的宽度和高度内进行绘制。

通过从已设定的宽度和高度分别减去边框和内边距才能得到内容的宽度和高度。

盒子的大小不会超过content规定的宽和高。**盒子大小=widith**

#### 盒子模型布局的稳定性

优先使用 宽度 width 其次 内边距 padding 最后 外边距 margin

原因：
1 margin会有外边距合并问题，其次ie6下margin会加倍（bug)

2 padding 会影响盒子的大小，需要进行加减运算

3 width 没有问题，可以使用宽度剩余法或者高度剩余法

宽度剩余法和高度剩余法：
利用盒子内容的宽高将其撑开

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/.2389pFjhhq*4s4zzElMnnmkmtff6nkGMVoqlphkTUA!/b/dEUBAAAAAAAA&bo=9AGpAQAAAAARF30!&rf=viewer_4) 

### 文档流

介绍文档流之前，先介绍元素的三种显示模式

#### 元素的显示模式

##### 块级元素(block-level)

每个块元素通常都会**独自占据一行或多整行**，可以对其设置宽度、高度、对齐等属性**(依然独自占据一行或多行）**。

`常见的块元素有<h1>~<h6>、<p>、<div>、<ul>、<ol>、<li>等，其中<div>标签是最典型的块元素`

块级元素的特点：

1 总是从新行开始

2 高度，行高，外边距、内边距都可以控制

3 宽度默认是容器的100%

4 可以容纳内联元素和其他块级元素

##### 行内元素(inline-level)

行内元素（内联元素）不占有独立的区域，仅仅依靠自身的字体大小和图像尺寸来支撑结构，一般不可以设置宽度、高度、对齐等属性，常用于控制页面中的文本样式。

`常见的行内元素有<a> <stong> <b> <em> <i> <del> <s> <ins> <u> <span>等，其中<span>是最典型的行内元素。`

行内元素的特点：

1 和相邻的行内元素在一行上。

2 高宽无效、但水平方向的padding和margin可以设置，垂直方向无效。

3 默认宽度就是它本身的宽度。（不能指定）

4 行内元素之内容纳文本或其它行内元素（a除外）

**注意：**

1 只用文字才能组成段落，因此p里面不能放块级元素，同理还有标题标签等，他们都是文字类块级标签，里面不能放其它块级元素。

2 链接里面不能放链接

##### 行内块元素(inline-block)

`在行内元素中有几个特殊的标签——<img> <input> <td> ，可以对它们设置宽高和对齐属性，有些资料称他们为行内块元素。`

行内块元素的特点：
1 和相邻行内元素（行内块）在一行上，但是之间会有空隙

2 默认宽度是它本身内容的宽度

3 高度、行高、外边距、以及内边距都可以控制

##### 模式的转换 display

dispaly: inline  块转行内

display: block  行内转块

display: inline-block 行、内转换为行内块

#### 概念

普通流或者标准流、文档流实际上就是一个网页内标签元素正常从上到下，从左到右排列顺序的意思，比如块级元素会独占一行，行内元素会按顺序依次前后排列；按这种大前提的布局排列之下绝对不会出现例外的情况叫做普通流布局。

### 定位

定位的基本思想很简单，它允许你定义元素框相对于其正常位置应该出现的位置，或者相对于父元素、另一个元素甚至浏览器窗口本身的位置。显然，这个功能非常强大，也很让人吃惊。要知道，用户代理对 CSS2 中定位的支持远胜于对其它方面的支持，对此不应感到奇怪。 

所以关键的找对相对于谁进行定位。

#### 静态定位（static）

 position的默认值，默认在文本流的位置。

一般用来清除定位。

#### 相对定位（relative）

**相对于自身进行定位**

1、相对定位时本身不脱离文本流。

2、每次移动是以自己的左上角为基点。

相对定位的的盒子仍在标准流里，它后面的盒子仍以标准流的方式对待它。

#### 绝对定位（absolute）

绝对定位是完全脱离标准流的。

**绝对定位是以依据最近已经定位（绝对、相对、固定）的父元素（祖先）进行定位的。如果祖先元素没有定位，会以body进行定。**

 使用技巧:子绝父相，保证父亲占有位置不影响下面的元素，还能保证子元素有定位的对象，以及飘在父元素的上方。

##### 绝对定位实现居中

加了absolute的盒子margin：0 auto；会失效。

左右居中：

1、首先left:50%(父盒子宽度的一半)

2、margin-left：自己的宽度一半（负值）

3、transfrom: translate(-50%) 可以不用知道元素的宽度 这个属性在动画里讲

上下居中同理。

#### 固定定位（fixed）

固定定位只认浏览器

相对于浏览器（视窗）进行定位

#### 叠加顺序

定位元素一定会覆盖没有定位的元素

`z-index`

1、默认值为0，取值越大，定位元素越居上。

2、只有绝对，相对，固定定位有此属性，其余标准流、浮动、静态定位都无此属性。

#### 定位转换

绝对定位、固定定位后会转换为行内块元素。

可以不用转化，直接给宽高。

### 浮动(float)

浮动刚开始的时候是用来做文字环绕图片的。

现在主要用于让多个盒子在同一行显示。

#### 什么是浮动？

元素的浮动是指设置了浮动属性的元素会脱离标准流的控制，移动到器父元素指定位置的过程。

`选择器{float:属性值;}`

| 属性值   | 描述         |
| ----- | ---------- |
| left  | 向左浮动       |
| right | 向右浮动       |
| none  | 元素不浮动（默认值） |

#### 浮动元素的特性

**浮动是脱离标准流的，不占位置，会影响标准流，浮动只有左右浮动。**

`浮动首先创建包含块的概念（包裹）。就是说浮动的元素总是找离它最近的父元素对齐。但是不会超过内边距的范围。`

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/APkWG.WU*4jHxKf9iZ2myWKmRH7fQs.h6OAnca7Kejw!/b/dFoAAAAAAAAA&bo=ggJWAQAAAAARF*c!&rf=viewer_4) 

#### 浮动的使用方法

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/t34462NXOse67XEzqEBHBM*QLReqDU0suF8V4ZOwQeY!/b/dEYBAAAAAAAA&bo=VAKBAQAAAAARF*Y!&rf=viewer_4) 

给两个黑色的盒子增加浮动，必须要有父盒子（黄色）将其包裹，防止因黑盒子浮动脱离文本流，蓝盒子上移被遮挡。

**浮动首先需要添加标准流父级**

`元素添加浮动后，元素会具有行内块元素的特性，元素的大小完全取决于定义的大小或者默认内容`

#### 清除浮动

#### 原因

由于浮动元素不再占用原文档流的位置，会对后面的元素排版产生影响，所以我们要清除浮动后产生的影响。

##### 清楚浮动本质

主要是为了父级元素因为子集浮动引起内部高度为0的问题。

`如果父元素没有设置高度，会由子元素撑起来，但当子元素浮动后，脱离了文本流，就会引起父元素的高度为0的问题`

##### 解决方法

再CSS中，clear属性用于清除浮动，其基本值有：

| 属性值   | 描述          |
| ----- | ----------- |
| right | 不允许右侧有浮动元素  |
| left  | 不允许左侧有浮动元素  |
| both  | 同时清除两侧的浮动影响 |

把浮动的盒子圈到里面，让父元素闭合出口，让其不能影响外部元素。

######  额外标签

在子元素最后增加一个盒子清除浮动。

```html
<style>
  .clear{
    clear:both;
  }
</style>
<div class="box">
        <div class="son1"></div>
        <div class="son1"></div>
        <div class="clear"></div>
    </div>
<div class="ex"></div>
```

###### 父级元素增加overflow

```html
<style>
   .box{
          overflow:hidden;
  	   }
</style>
<div class="box">
        <div class="son1"></div>
        <div class="son1"></div>
        <div class="clear"></div>
    </div>
<div class="ex"></div>
```

缺点：内容增多时容易造成不会自动换行导致内容被隐藏掉，无法显示需要溢出的元素。

###### 使用after伪元素清除浮动

```css
.clearfix:after{
  content:".";/*防止就浏览器有空隙*/
  display:block;
  height:0;
  clear:both;
  visibility:hidden;/*隐藏盒子*/
}
.clearfix{
	*zoom:1;/*兼容IE6、7*/
}
<div class="box1 clearfix">
        <div class="son1"></div>
        <div class="son1"></div>
    </div>
```

与增加盒子法原理一样

###### 使用双伪元素清除浮动

```css
.clearfix:before,
clearfix::after{
            content: "";
            display: table;
        	   }
.clearfix:after{
              clear:both;
}
.clearfix{
	*zoom:1;/*兼容IE6、7*/
}
```

### flex布局

来源：http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html 

#### 一、Flex布局是什么？

Flex是Flexible Box的缩写，意为”弹性布局”，用来为盒状模型提供最大的灵活性。

任何一个容器都可以指定为Flex布局。

```
.box{
  display: flex;
}
```

行内元素也可以使用Flex布局。

```
.box{
  display: inline-flex;
}
```

Webkit内核的浏览器，必须加上-webkit前缀。

```
.box{
  display: -webkit-flex; /* Safari */
  display: flex;
}
```

注意，设为Flex布局以后，子元素的float、clear和vertical-align属性将失效。

#### 二、基本概念

采用Flex布局的元素，称为Flex容器（flex container），简称”容器”。它的所有子元素自动成为容器成员，称为Flex项目（flex item），简称”项目”。

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/dc13Q.m9Qj6OauNAdq02N6Z1ABTHyplMAsCAw4oNXA0!/b/dDIBAAAAAAAA&bo=MwJNATMCTQEDCSw!&rf=viewer_4) 

容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end。

项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。

#### 三、容器的属性

以下6个属性设置在容器上。

> - flex-direction
> - flex-wrap
> - flex-flow
> - justify-content
> - align-items
> - align-content

##### flex-direction属性

flex-direction属性决定主轴的方向（即项目的排列方向）。

```
.box {
  flex-direction: row | row-reverse | column | column-reverse;
}
```

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/qhQ8cNkSt075mV28Py7KVi0J2UdtRwF0E.ZShGXY230!/b/dC8BAAAAAAAA&bo=HAPLABwDywADCSw!&rf=viewer_4) 

它可能有4个值。

> - row（默认值）：主轴为水平方向，起点在左端。
> - row-reverse：主轴为水平方向，起点在右端。
> - column：主轴为垂直方向，起点在上沿。
> - column-reverse：主轴为垂直方向，起点在下沿。

##### flex-wrap属性

默认情况下，项目都排在一条线（又称”轴线”）上。flex-wrap属性定义，如果一条轴线排不下，如何换行。

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/vmOqnwd7femAslW7caQV82t0fkvOdUFiJP5B2Qy9JLg!/b/dAgBAAAAAAAA&bo=HgMUAR4DFAEDCSw!&rf=viewer_4) 

```
.box{
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```

它可能取三个值。

（1）nowrap（默认）：不换行。

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/yajBhvZsfvfH*Yn582hJaFLsNwaXmYDcNhiI*CxnI9c!/b/dFUAAAAAAAAA&bo=vAKRALwCkQADCSw!&rf=viewer_4) 

（2）wrap：换行，第一行在上方。

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/lJSvkjr9BFYnFhM1VpUp1qL8GzmKt0PFTTcJoWC.Njg!/b/dAgBAAAAAAAA&bo=vAKxALwCsQARCT4!&rf=viewer_4) 

（3）wrap-reverse：换行，第一行在下方。

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/H0h*7F9R8Utgn41PfT2pCWprsS71qufgpY21tsZJWDg!/b/dDEBAAAAAAAA&bo=vAKxALwCsQARCT4!&rf=viewer_4) 

##### flex-flow

flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。

```
.box {
  flex-flow: <flex-direction> || <flex-wrap>;
}
```

##### justify-content属性

justify-content属性定义了项目在主轴上的对齐方式。

```
.box {
  justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/X19N.jOesWKE2NDT1x75nsHcGUp*z6s.EUHDNZXw1m4!/b/dDMBAAAAAAAA&bo=fQL7An0C.wIDCSw!&rf=viewer_4) 

它可能取5个值，具体对齐方式与轴的方向有关。下面假设主轴为从左到右。

> - flex-start（默认值）：左对齐
> - flex-end：右对齐
> - center： 居中
> - space-between：两端对齐，项目之间的间隔都相等。
> - space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

##### align-items属性

align-items属性定义项目在交叉轴上如何对齐。

```
.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
```

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/enFrVW6jrdNUp1HhA7wQrBZdMryKX5MxXdeR3l9lL1c!/b/dFYAAAAAAAAA&bo=aQISA2kCEgMDCSw!&rf=viewer_4) 

它可能取5个值。具体的对齐方式与交叉轴的方向有关，下面假设交叉轴从上到下。

> - flex-start：交叉轴的起点对齐。
> - flex-end：交叉轴的终点对齐。
> - center：交叉轴的中点对齐。
> - baseline: 项目的第一行文字的基线对齐。
> - stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

##### align-content属性

align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

```
.box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/sDXSWUK5TWpxic6w3z1Q*waQiE3qW3HNCzYCG2RKW00!/b/dDMBAAAAAAAA&bo=bAISA2wCEgMDCSw!&rf=viewer_4) 

该属性可能取6个值。

> - flex-start：与交叉轴的起点对齐。
> - flex-end：与交叉轴的终点对齐。
> - center：与交叉轴的中点对齐。
> - space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
> - space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
> - stretch（默认值）：轴线占满整个交叉轴。

#### 四、项目的属性

以下6个属性设置在项目上。

> - order
> - flex-grow
> - flex-shrink
> - flex-basis
> - flex
> - align-self

##### order属性

order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。

```
.item {
  order: <integer>;
}
```

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/MVY1gv7zi.ILaIN6n*EoF5NaAIIXPnY042bOtiexrXg!/b/dDEBAAAAAAAA&bo=7wLgAe8C4AEDCSw!&rf=viewer_4) 

##### flex-grow属性

flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

```
.item {
  flex-grow: <number>; /* default 0 */
}
```

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/D6K1VtmRuHzeIDtF3cRMUy38BrJxadcFe51IeFMgLDk!/b/dGYBAAAAAAAA&bo=IgPTACID0wADCSw!&rf=viewer_4) 

如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

##### flex-shrink属性

flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

```
.item {
  flex-shrink: <number>; /* default 1 */
}
```

![img](http://r.photo.store.qq.com/psb?/V13oJHnV4UEUs2/U0UgfW6fiits5OY8T6*swKZ0*.2iR.ocPsQ.SGmAHVY!/r/dGcBAAAAAAAA) 

如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。

负值对该属性无效。

##### flex-basis属性

flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。

```
.item {
  flex-basis: <length> | auto; /* default auto */
}
```

它可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间。

#####  flex属性

flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。

```
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```

该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)。

建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。

##### align-self属性

align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

```
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/GcDp2z7xHFFevi9Kcsk1zXK7QWLbDz8LmfeV*N6CQR0!/b/dEcBAAAAAAAA&bo=5wKGAecChgEDCSw!&rf=viewer_4) 

该属性可能取6个值，除了auto，其他都与align-items属性完全一致。

### grid布局

来源：http://www.css88.com/archives/8506

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/eFpW*SDE807Vhtk912udOrYQ3fpDVf7Te528zVsEIno!/b/dFkAAAAAAAAA&bo=6AMOAegDDgEDCSw!&rf=viewer_4) 

Grid 布局是网站设计的基础，CSS Grid 是创建网格布局最强大和最简单的工具。

CSS Grid 今年也获得了主流浏览器（Safari，Chrome，Firefox，Edge）的原生支持，所以我相信所有的前端开发人员都必须在不久的将来学习这项技术。

在本文中，我将尽可能快速地介绍CSS网格的基本知识。我会把你不应该关心的一切都忽略掉了，只是为了让你了解最基础的知识。

#### 你的第一个 Grid 布局

CSS Grid 布局由两个核心组成部分是 **wrapper**（父元素）和 **items**（子元素）。 wrapper 是实际的 grid(网格)，items 是 grid(网格) 内的内容。

下面是一个 wrapper 元素，内部包含6个 items ：

```html
HTML 代码:
<div class="wrapper">  
<div>1</div>  
<div>2</div>  
<div>3</div>  
<div>4</div>  
<div>5</div>  
<div>6</div>
</div>
```

要把 wrapper 元素变成一个 grid(网格)，只要简单地把其 `display` 属性设置为 `grid` 即可：

```
CSS 代码:
.wrapper {    
display: grid;}
```

但是，这还没有做任何事情，因为我们没有定义我们希望的 grid(网格) 是怎样的。它会简单地将6个 div 堆叠在一起。

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/H*SX3mAZURu6SlIARD2yimlbdZW1hREIQZ.Bbz6*Aq8!/b/dFUAAAAAAAAA&bo=IAPrACAD6wADCSw!&rf=viewer_4) 

我已经添加了一些样式，但是这与 CSS Grid 没有任何关系。

#### Columns(列) 和 rows(行)

为了使其成为二维的网格容器，我们需要定义列和行。让我们创建3列和2行。我们将使用`grid-template-row`和`grid-template-column`属性。

```css
CSS 代码:.wrapper {    
display: grid;    
grid-template-columns: 100px 100px 100px;    
grid-template-rows: 50px 50px;}
```

正如你所看到的，我们为 `grid-template-columns` 写入了 3 个值，这样我们就会得到 3 列。 我们想要得到 2 行，因此我们为 `grid-template-rows` 指定了2个值。

这些值决定了我们希望我们的列有多宽（ `100px` ），以及我们希望行数是多高（ `50px` ）。 结果如下：

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/91X.PxhLTlDcoP4tOuuo7At3o3D5QHVkJBOYerAJo34!/b/dFkAAAAAAAAA&bo=IAMoASADKAEDCSw!&rf=viewer_4) 

为了确保你能正确理解这些值与网格外观之间的关系，请看一下这个例子。

```
CSS 代码:
.wrapper {    
display: grid;    
grid-template-columns: 200px 50px 100px;    
grid-template-rows: 100px 30px;}
```

请尝试理解上面的代码，思考一下以上代码会产生怎样的布局。

这是上面代码的布局的结果：

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/0EEbvXVIWhYBaQE*Po6N231bsqqoARpAYGzpjwKvhOQ!/b/dDABAAAAAAAA&bo=IAPJACADyQADCSw!&rf=viewer_4) 

非常好理解，使用起来也非常简单是不是？下面我们来加大一点难度。

#### 放置 items(子元素)

接下来你需要学习的是如何在 grid(网格) 上放置 items(子元素) 。特别注意，这里才是体现 Grid 布局超能力的地方，因为它使得创建布局变得非常简单。

我们使用与之前相同的 HTML 标记，为了帮助我们更好的理解，我们在每个 items(子元素) 加上了单独的 `class` ：

```
HTML 代码:
<div class="wrapper">  
<div class="item1">1</div>  
<div class="item2">2</div>  
<div class="item3">3</div>  
<div class="item4">4</div>  
<div class="item5">5</div>  
<div class="item6">6</div>
</div>
```

现在，我们来创建一个 3×3 的 grid(网格)：

```
CSS 代码:.wrapper {    
display: grid;    
grid-template-columns: 100px 100px 100px;    
grid-template-rows: 100px 100px 100px;}
```

将得到以下布局：

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/91X.PxhLTlDcoP4tOuuo7At3o3D5QHVkJBOYerAJo34!/b/dFkAAAAAAAAA&bo=IAMoASADKAEDCSw!&rf=viewer_4) 

不知道你发现没有，我们只在页面上看到 3×2 的 grid(网格)，而我们定义的是 3×3 的 grid(网格)。这是因为我们只有 6 个 items(子元素) 来填满这个网格。如果我们再加3个 items(子元素)，那么最后一行也会被填满。

要定位和调整 items(子元素) 大小，我们将使用 `grid-column` 和 `grid-row` 属性来设置：

```
CSS 代码:
.item1 {    
grid-column-start: 1;    
grid-column-end: 4;}
```

我们在这里要做的是，我们希望 item1 占据从第一条网格线开始，到第四条网格线结束。换句话说，它将独立占据整行。 以下是在屏幕上显示的内容：

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/3qQtdjqcCHnFDR4y*BSTBTzRxUd9krGvwpX2yw27B4s!/b/dDIBAAAAAAAA&bo=IANSASADUgEDCSw!&rf=viewer_4) 

如果你不明白我们设置的只有 3 列，为什么有4条网格线呢？看看下面这个图像，我画了黑色的列网格线：

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/.55HOQKkUF5wJ*OB13uYjzZKs555Eendy*e77tGhkT4!/b/dFoAAAAAAAAA&bo=IANSASADUgEDCSw!&rf=viewer_4) 

请注意，我们现在正在使用网格中的所有行。当我们把第一个 items(子元素) 占据整个第一行时，它把剩下的 items(子元素) 都推到了下一行。

最后，给你一个更简单的缩写方法来编写上面的语法：

```
CSS 代码:
.item1 {    
grid-column: 1 / 4;}
```

为了确保你已经正确理解了这个概念，我们重新排列其他的 items(子元素) 。

```css
CSS 代码:
.item1 {    
grid-column-start: 1;    
grid-column-end: 3;}
.item3 {    
grid-row-start: 2;    
grid-row-end: 4;}
.item4 {    
grid-column-start: 2;    
grid-column-end: 4;}
```

你可以尝试在你的脑子里过一边上面代码的布局效果，应该不会很难。

以下是页面上的布局效果：

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/9R6hhjdBhLCJldq2q6OqA8zky*agrnAPk1yUs9S*ODY!/b/dEABAAAAAAAA&bo=IANSASADUgEDCSw!&rf=viewer_4) 

Grid 布局就是这么简单，当然这里展示的是最简单的 Grid 布局概念，但是 Grid 布局系统中还有更多强大灵活的特性。作为本文续篇，请阅读 [如何使用 CSS Grid 快速而又灵活的布局](http://www.css88.com/archives/8512) 让你体会 Grid 布局真正的强大和灵活。在此之前建议阅读请[CSS Grid 布局完全指南(图解 Grid 详细教程)](http://www.css88.com/archives/8510)，首先了解一下 Grid 相关的术语和所有属性。

## 常用样式

### 字体

#### font-size 字体大小

font-size属性用于设置字号，该属性可以使用相对长度单位，也可以使用绝对长度单位，常用px。

#### font-family 字体

font-family用于设置字体，网页中常用的字体有宋体、微软雅黑、黑体等。

可以同时指定多个字体，中间以逗号隔开，表示如果浏览器不支持第一个字体,则会尝试下一个字体。

```txt
中文字体需要加英文状态下的引号，英文字体一半不需要加引号，当设置英文字体时，英文字体必须位于中文字体名之前
如果字体名中包含空格，#，$等符号，则该字体必须加英文状态下的单引号或双引号，列如：font-family:"Time New Roman"
```

#### CSS unicode

简单来说，用编码表示字体。

防止因文件编码不匹配产生乱码。

| 中文名     | 英文名             | unicode                      | unicode2                                   |
| ---------- | ------------------ | ---------------------------- | ------------------------------------------ |
| 宋体       | SimSun             | `\5B8B\4F53`                 | `&#x5B8B;&#x4F53;`                         |
| 黑体       | SimHei             | `\9ED1\4F53`                 | `&#x9ED1;&#x4F53;`                         |
| 新宋体     | NSimSun            | `\65B0\5B8B\4F53`            | `&#x65B0;&#x5B8B;&#x4F53;`                 |
| 楷体       | KaiTi              | `\6977\4F53`                 | `&#x6977;&#x4F53;`                         |
| 微软正黑体 | Microsoft JhengHei | `\5FAE\x8F6F\6B63\9ED1\4F53` | `&#x5FAE;&#x8F6F;&#x6B63;&#x9ED1;&#x4F53;` |
| 微软雅黑   | Microsoft YaHei    | `\5FAE\8F6F\96C5\9ED1`       | `&#x5FAE;&#x8F6F;&#x96C5;&#x9ED1;`         |

参考资料：https://zhuyujia.github.io/css/css-font-unicode-table.html

#### 字体强调

##### font-weight 字体粗细

字体加除了用b和strong标签外，可以用font-weight实现。

`font-weight用于定义字体的粗细，其属性值可以为normal、bold、bloder、lighter、100~900（100的整数），其中400等价于 normal，700等价于bold`

##### font-style 字体风格

字体倾斜除了用i和em外，可以用font-style实现。

其属性值有：

- normal 默认值，浏览器会显示标准的字体样式。
- italic 斜体
- oblique 倾斜

##### 综合设定

`{font: font-style font-weihgt font-size/line-height font-family}`

使用font属性时，必须按照以上顺序书写，不能更换顺序，各个属性之间一空格隔开。

其中不需要设置的属性可以忽略，但必须保留font-size和font-family属性

#### 文字阴影（CSS3）

`text-shadow:水平位置 垂直位置 模糊距离 阴影颜色`

```css
text-shadow: h-shadow v-shadow blur color;
```

注释：text-shadow 属性向文本添加一个或多个阴影。该属性是逗号分隔的阴影列表，每个阴影有两个或三个长度值和一个可选的颜色值进行规定。省略的长度是 0。

| 值         | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| *h-shadow* | 必需。水平阴影的位置。允许负值。                             |
| *v-shadow* | 必需。垂直阴影的位置。允许负值。                             |
| *blur*     | 可选。模糊的距离。                                           |
| *color*    | 可选。阴影的颜色。参阅 [CSS 颜色值](http://www.w3school.com.cn/cssref/css_colors_legal.asp)。 |

参考资料：http://www.w3school.com.cn/cssref/pr_text-shadow.asp
### 背景

CSS可以添加背景颜色和背景图片，以及来进行图片设计。

| background-color      | 背景颜色      |
| --------------------- | --------- |
| background-image      | 背景图片      |
| background-repeat     | 是否平铺      |
| background-position   | 背景位置      |
| background-attachment | 背景是固定还是滚动 |

背景的合写

background: 颜色 图片地址 背景平铺 背景滚动 背景位置

#### 背景位置

默认值 ： 左上角

```css
background-position:length||length

background-position:position||positiion
```

参数：

length：百分数 数字值

position：top、center、bottom、left、right 方位名词没有顺序

设置或检索对象的背景图像位置。必须先指定background-image属性。默认值为（0% 0%）

如果只设定了一个值，该值将用于横坐标。纵坐标默认为50%。第二个值用于纵坐标。

注意：

1.postion 后面是x坐标和y坐标。可以使用方位名词或者精确单位。

2.如果精确单位和方位名词混合使用，必须是x坐标在前，y坐标在后面。

```css
background-postion:15px top;
```

#### 背景附着

```css
background-attachment:scroll||fixed;
```

scroll: 背景图像是随内容滚动（默认值）

fixed: 背景图像固定

#### 背景透明

使用rgba(0,0,0,透明度)

#### 背景缩放

background-size设置图片的尺寸

a、可以设置长度单位（px)或百分比（参照盒子的宽高）

b、设置为cover时，会自动调整比例，保证图片填满背景区，如有溢出则会被隐藏。

c、设置为contain会自动调整比例，保证图片会完整的显示在背景区域（只要宽或高有一个达到了盒子的大小，则不会进行缩放,保证了图片完整）

#### 多背景（CSS3）

以逗号分隔可以设置多背景

- 一个元素可以设置多重背景图像
- 每组属性间用逗号隔开（定义属性的时候使用简写形式）
- 如果设置的多重背景图之间存在着交集（重叠关系），前面的背景图会覆盖在后面的背景图上
- 为了避免背景色将图像盖住，背景色通常为最后一组

```css
background:url(XXX.png) no-repeat scroll 10px 20px,
url(YYY.png) no-repeat scroll left bottom;
```
### 显示与隐藏

#### display

display 设置检索对象如何显示。

dispay：none; 隐藏元素，隐藏之后，**不在保留位置**。

display：block；除了有转换为块级元素之外，还有显示元素的意思。

经常用于菜单。

#### visibilty

visibilty:hidden;隐藏元素，**并保留位置**。

visibilty：visible; 显示元素。

#### overflow

管理内容超出指定宽高时的做法。

| 值      | 说明                               |
| ------- | ---------------------------------- |
| visible | 默认值，超出显示                   |
| auto    | 自动，超出显示滚动条               |
| scroll  | 一直显示滚动条（无论内容是否超出） |
| hidden  | 超出部分隐藏                       |

### 用户界面

#### 鼠标样式 cursor

设置或检索在对象上移动鼠标指针采用何种系统预定义的光标形状。

`cursor: default 小白(箭头）|pointer 小手|move 移动（十字标）|text 文本（光标）`

#### 轮廓 outline

是绘制元素周围的一条线，位于边框的外围，可以起到突出元素的作用。

`outline:outline-color||outline-style||outline-width` (类似与border的设置)

我们平时都是去掉边框

写法为：outline:0;

```html
<input type="text" style="outline:0">
```

#### 防止拖拽文本域 resize

resize: none 防止用户随意拖拽文本域

```html
<textarea style="resize:none;outline:0"></textarea>
```

#### 垂直对齐 vertical-align

vertical-align **不影响块级元素中的内容对齐**，它只针对与行内元素或者行内块元素，特别是行内块元素，通常用来控制图片和表单文字的对齐。

`vertical-align:baseline |top| middle| bottom`

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/Jo3pz56vDi.KeSmMDo8gdFc4jIIqx*M57dP5XY.1hQo!/b/dC0BAAAAAAAA&bo=sQPGAAAAAAARF1Q!&rf=viewer_4) 

通常默认是基线对齐，比如：

```html
 <textarea name="" id="" cols="30" rows="10"></textarea>
    <p style="display:inline-block">文字文字文字文字</p>
```

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/oFCaEBKn7vtA2wA0FQQqUfOG*e2jZ4v*okyQ0ql9ee0!/b/dEABAAAAAAAA&bo=rwHdAAAAAAARF1M!&rf=viewer_4) 

给文本域增加属性,使其与中线对齐

```html
<textarea name="" id="" cols="30" rows="10" style="vertical-align:middle"></textarea>
    <p style="display:inline-block">文字文字文字文字</p>
```

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/VwtJovK5v.iSn6aajHqxr6TJBeRtWX0JbHG7wd9sG.k!/b/dDEBAAAAAAAA&bo=xAHgAAAAAAARFwU!&rf=viewer_4) 

#### 去除图片底侧空白间隙

图片或者表单等行内块元素，**它的底线会和父盒子的基线对齐**，这样会造成图片底侧会有一个空白缝隙。

解决方案：
1 将行内块元素转换块级元素， display:block;

2 给行内块元素添加 vertical-align:middle || top 等等， 让行内块元素不要和基线对齐。

####溢出文字隐藏

##### word-break 自动换行

**主要处理英语**

normal 浏览器的默认换行规则

break-all 允许在单词内换行（任何时候,可以拆开单词）

keep-all 只能在半角空格或连字符处换行（无法拆开单词，除非出现连字符“-”）

##### white-space

设置或检索对象内文本的显示方式，通常我们使用与强制一行显示内容

normal: 默认处理方式

nowrap: 强制在同一行显示所有文本，直到文本结束或者遭遇br标签

##### text-overflow 文字溢出

text-overflow: clip||ellipsis

设置是否使用一个省略标记(...)表示对象文本的溢出

clip 不显示省略标记，而是简单的裁切

ellipsis 溢出时用省略号标记

**注意使用条件**

一定要首先强制一行内显示，再次和overflow属性搭配使用

```css
        white-space: nowrap;/*首先添加这句话，强制不换行*/
        overflow: hidden;/*必须有这句话，溢出部分隐藏*/
        text-overflow: ellipsis;
```
#### 字体图标

图片是有诸多优点的，但是图片不能进行良好的“缩放”，因为图片的放大和缩小会失真，而且还增加了总文件的大小。但是字体图标可以改善这些缺点。

##### 优点

```txt
可以做出和图片一样的事情，改变透明度，旋转度，等
但是本质是文字，可以改变颜色，产生阴影，透明效果等....
体积小，但是所携带的信息没有消减
几乎支持所有的浏览器
```

##### 使用

##### 上传生成字体包

当我们有.svg文件时，我们需要转换成我们页面能使用的字体文件，而且需要生成的是兼容性的适合各个浏览器的。

推荐网站：http://icomoon.io

阿里字库

http://www.icofont.cn/

##### icomoon的使用

1 将font文件夹放到目录文件夹上

2 在样式中声明字体

在下载的文件夹中style文件中复制如下的代码放进css中

**注意路径问题**

比如：

```css
@font-face {
  font-family: 'icomoon';
  src:  url('fonts/icomoon.eot?1adt26');
  src:  url('fonts/icomoon.eot?1adt26#iefix') format('embedded-opentype'),
    url('fonts/icomoon.ttf?1adt26') format('truetype'),
    url('fonts/icomoon.woff?1adt26') format('woff'),
    url('fonts/icomoon.svg?1adt26#icomoon') format('svg');
  font-weight: normal;
  font-style: normal;
}
```

3 增加span标签，并添加样式`font-family: "icomoon"` （要与上面声明的字体样式名字一样）

4 将demo中样式后面的小方格复制到span标签中

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/KYSKRlD6P51zTd6SjufPJeD66aIqHVeINr65XcjvMnM!/b/dFYAAAAAAAAA&bo=iQKqAAAAAAARFwE!&rf=viewer_4) 

或增加样式`span::before{content:"\e906"}` (字体图标下面的代码)

##### 追加新图标

将压缩包中的selection.json从新上传，然后选中自己想要的新图标，从新下载压缩包，替换原来的文件即可。

## 动画

### 过渡

过渡三要素:

1 必须要有属性发生变化

2 必须告诉系统哪个属性发生了变化 transition-property

3 必须告诉系统过渡效果持续的时长 transition-duration

例如：

```css
.gd{
    width: 300px;
    height: 200px;
    background-color: red;
    /* 告诉系统哪个属性需要执行过渡效果 */
    transition-property: width,background-color;
    /* 告诉系统过渡效果所用的时间 */
    transition-duration: 5s, 6s;
}
/* 伪类选择器也可以用在不是a标签上，只是表示监听鼠标指向元素的事件 */
.gd:hover{
    width: 500px;
    background-color: blue;
}
```

其它的过渡属性：

transition-delay 规定了过渡效果何时开始

transition-timing-function 规定了过渡的运动曲线

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/GqVFpSj2fKX9CzPzW3YwXl5m.8ddAM90OcTZTuYERbA!/b/dIMAAAAAAAAA&bo=pwNDAQAAAAARF8Y!&rf=viewer_4) 

属性的连写：

transition: property duration timing-function delay;

transition: 过渡属性 过渡时长 运动时间函数 延迟

如果想给多个属性增加过渡效果用逗号隔开

连写的时候可以省略后两个参数

如果多个属性运动的速度/延迟的时间/持续的时间都一样，可以简写为：

`transition:all 0s;`

如何编写过渡：

1 不要管过渡，先编写基本界面

2 修改我们认为需要修改的属性

3回过头去给被修改属性的那个元素添加过渡效果

### 2D转换

通过 CSS3 转换，我们能够对元素进行移动、缩放、转动、拉长或拉伸。

#### 基本属性 transform

##### rotate() 方法

通过 rotate() 方法，元素顺时针旋转给定的角度。允许负值，元素将逆时针旋转。

```css
.d2 ul li:nth-child(2){
    /* 其中deg是单位，代表多少度 */
    transform: rotate(45deg);
}
```

##### translate() 方法

通过 translate() 方法，元素从其当前位置移动，根据给定的 left（x 坐标） 和 top（y 坐标） 位置参数：

```css
.d2 ul li:nth-child(3){
    /* 第一个参数:水平方向
    第二个参数：垂直方向 */
    transform: translate(100px, 50px);
}
```

##### scale() 方法

通过 scale() 方法，元素的尺寸会增加或减少，根据给定的宽度（X 轴）和高度（Y 轴）参数：

```css
.d2 ul li:nth-child(4){
    /* 第一个参数:水平方向
    第二个参数：垂直方向 
    如果取值为1， 代表不变
    取值大于1 放大
    取值小于1 缩小
    如果水平和垂直的参数都一样，可以简写为一个参数*/
    transform: scale(1.5,1.5);
}
```

如果需要进行多个转换，要用空格隔开

2D转化模块会修改元素的坐标系，所以旋转之后的平移就不是水平平移了

#### 形变中心点

默认情况下所有的元素都是以自己的中心点作为参考来旋转的，我们可以通过形变中心点属性来修改它的参考点

transform-origin：0px 0px;

第一个参数:水平方向

第二个参数:垂直方向

注意点:

取值有三种形式：具体像素 百分比  特殊关键字(left center top等)

#### 旋转轴向

默认情况下所有元素都是围绕Z轴进行旋转

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/Kz8NUQJIBHCEx.pF5xw5CthHk*TWCs0P8QLXTuEAMAg!/b/dEcBAAAAAAAA&bo=BAKsAQAAAAARF4s!&rf=viewer_4) 

Z轴的正方向为垂直屏幕向外

想围绕哪个轴旋转, 那么只需要在rotate后面加上哪个轴即可

比如rotateX(45deg) rotateY(-45deg) rotate Z(180deg)

透视：perspective: 500px;

呈现一种近大远小的效果,一般和旋转搭配使用

注意点:一定要注意, 透视属性必须添加到需要呈现近大远小效果的元素的**父元素上面**

#### 注意

过渡尽量:hover的元素和改变的元素是同一个，通过A:hover去控制B的也可以用CSS做，具体做法是：
A是父级，B是子集，那么A:hover >B{}就可以了（认为A:hover时找到B）

AB同级时，A:hover ~B(A在HTML中在B的上面，认为A:hover时找到B)

CSS 相邻兄弟选择器： + （A:hover +B) 选择**紧接**在另一个元素后的元素，而且二者**有相同的父元素**

~  在使用 ~ 连接两个元素时,它会**匹配第二个元素**,条件是它必须跟(**不一定是紧跟**)在第一个元素之后,且他们都有一个共同的父元素.

有一个过渡显示二级菜单的例子是通过:hover一级菜单的li去显示二级菜单的li的例子

代码：https://paste.ubuntu.com/p/84j55Xws62/

### 3D转换

关键：

transform-style： preserve-3d  //子元素将保留其 3D 位置 

默认值为flat 即为2D转换

实例：画一个正方体

先画六个面，通过旋转和位移将面围成正方体，**需要注意的是，当面进行旋转操作的时候，对于这个元素的坐标系会跟着一起旋转**

具体可以看这个代码就明白了，

https://paste.ubuntu.com/p/V6YwZp6HJV/

![img](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/wFPSePlx921PWj*YvFIySrdmIjCz0x0diMlXrRS4WHw!/b/dC8BAAAAAAAA&bo=1gHeAdYB3gECia0!&rf=viewer_4) 

### 动画

过渡与动画的不同点：

过渡必须人为的触发才会执行

动画不需要人为的触发就可以执行动画

相同点：

过渡和动画都需要满足三要素

动画的用法：
1 先告诉系统需要执行哪个动画

animation-name: lxx(随便给动画起个名称)；

2 告诉系统我们要创建一个名称为lxx的动画

```css
@keyframes lxx{
	from{
	默认态
}
	to{
	结束状态
}
}
```

动画的过程也可以用%表示

```css
@keyframe lxx{
    0%{
        
    }
    33%{
        
    }
    66%{
        
    }
    100%{
        
    }
}
```



3 告诉系统动画持续的时长

animation-duration:3s;

#### 其它属性

animation-iteration-count: 3;

动画需要执行几次

animation-direction：alternate;动画应该轮流反向播放

默认值：normal 动画不执行往返

animation-play-state: runing/paused

规定动画是否暂停（与hover联合使用）

animation-fill-mode

| 值        | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| none      | 不改变默认行为。(恢复没有动画的状态)                         |
| forwards  | 当动画完成后，保持最后一个属性值（在最后一个关键帧中定义）。 |
| backwards | 在 animation-delay 所指定的一段时间内，在动画显示之前，应用开始属性值（在第一个关键帧中定义）。（让动画在等待状态时显示第一帧的样子） |
| both      | 向前和向后填充模式都被应用。（forward和backwards一起）       |

#### 连写

animation: name duration timing-function delay iteration-count direction;

