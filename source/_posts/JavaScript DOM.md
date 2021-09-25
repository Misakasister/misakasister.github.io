---
title: JavaScript DOM
tags:
- 前端
- JavaScript
categories: 编程
---

## 概念

DOM：文档对象模型

![BOM](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/GMV50l6L*sNffUSZbHodd1Lzz6H8*tmFKT46RV063N8!/b/dFUAAAAAAAAA&bo=6AKZAQAAAAARB0I!&rf=viewer_4)

DOM--> Document Object Model

DOM定义了表示和修改文档所需的方法。DOM对象即为宿主对象，由浏览器厂商定义，用来操作**html和xml**（没有CSS样式表）功能的一类对象的集合。

## 获取元素

**注意：节点与元素节点不一样，节点包含元素节点，注释节点，文本节点等等。**

获取元素节点

| 名称                      | 解释                                                         |
| ------------------------- | ------------------------------------------------------------ |
| document.getElementById() | 元素id                                                       |
| getElementsByTagName()    | 标签名 返回类数组  实时的选择，包括后面增加的                |
| getElementsByName()       | 只有部分标签name可生效(表单 表单元素 img iframe) 返回类数组  |
| getElementsByClassName()  | 类名 ie8(含)以下版本没有                                     |
| querySelector()           | CSS选择器 ie7(含) 以下没有 非实时的选择（后面新加的无法选择） |
| querySelectorAll()        | CSS选择器 ie7(含) 以下没有 非实时的选择（后面新加的无法选择） |

基于元素节点的遍历

| 名称                                           | 解释                                            |
| ---------------------------------------------- | ----------------------------------------------- |
| parentElement                                  | 返回当前元素的父节点（ie9（含）以下 不兼容）    |
| children                                       | 只返回当前元素元素子节点                        |
| node.childElementCount ===node.children.length | 当前元素节点的子元素个数 ie9（含）以下 不兼容） |
| firstElementChild                              | 返回第一个元素节点 ie9（含）以下 不兼容）       |
| lastElementChild                               | 返回的是最后一个元素节点                        |
| nextElementSibling/previousElementSlibing      | 返回后一个/前一个兄弟元素                       |

### 节点属性

通过节点.属性访问

| 名称            | 解释                                                       |
| --------------- | ---------------------------------------------------------- |
| nodeName        | 元素的标签名，以大写表示，只读                             |
| nodeValue       | Text节点或Comment节点的文本内容，可读写                    |
| nodeType        | 该节点类型 ，只读                                          |
| attributes      | Element 节点的属性集合   attributes[0].value可以获得属性值 |
| hasChildNodes() | 判断有没有子节点                                           |

## 基本操作

### 增

| 名称                              | 解释             |
| --------------------------------- | ---------------- |
| document.createElement()          | 创建元素节点     |
| document.createTextNode()         | 创建文本节点     |
| document.createComment()          | 创建注释节点     |
| document.createDocumentFragment() | 创建文档片段对象 |

### 插

增加后插入才能在html中显示

| 名称                         | 解释                                                      |
| ---------------------------- | --------------------------------------------------------- |
| parentNode.appendChild()     | 最后一位插入，相当于push() 把页面已有部分插入，相当于剪贴 |
| parentNode.insertBefore(a,b) | a在b之前插入                                              |

### 删

| 名称                 | 解释             |
| -------------------- | ---------------- |
| parent.removeChild() | 父节点删除子节点 |
| chid.remove()        | 子节点将自己删除 |

### 替换

| 名称                            | 解释 |
| ------------------------------- | ---- |
| parent.replaceChild(new,origin) | 节点 |

### 对属性的修改

Element 节点的一些属性

| 名称      | 解释                                                   |
| --------- | ------------------------------------------------------ |
| innerHTML | 改变html内容                                           |
| innerText | 改变文字内容（老版本火狐不兼容） textContent(IE不好使) |

Element 节点的一些方法

| 名称                   | 解释            |
| ---------------------- | --------------- |
| ele.setAttribute(a，b) | a属性名 b属性值 |
| else.getAttribute(a)   | a属性名         |

## date对象 与 定时器

### Date 对象

Date 对象用于处理日期和时间。

创建 Date 对象的语法：

```
var myDate=new Date()
```

注释：Date 对象会自动把当前日期和时间保存为其初始值。（时间戳）

自定义时间：

new Date(year, month, day，hour, minute, second)

**month从零开始计数**

date所用的方法：http://www.w3school.com.cn/jsref/jsref_obj_date.asp

### 循环定时器

**setInterval()** ：按照指定的周期（以毫秒计）来调用函数或计算表达式。方法会不停地调用函数， 

两个参数： 函数体 时间（毫秒）

使用时注意事项：与事件绑定时，为了防止重复触发，首先要清除定时器，使用 clearInterval()  ，参数为setInterval的返回值（可以用变量接收一下）

### 延迟定时器

当方法执行完成定时器停止(但是定时器还在,只不过没用了);

var tmid = window.setTimeout(“方法名或方法”, “延时”);

window.clearTimeout(tmid);

## 获得窗口尺寸

### 获取滚动条滚动距离

**scrollTop** 

网页被卷去的高

兼容问题：

ie9+

window.pageOffset;

火狐和其它浏览器

doucument.documentElement.scrollTop

谷歌和没有声明 DTD\<DOCTYPE\>

document.body.scrollTop

**scrollTo(x,y)** 

把内容滚动到指定的坐标，一般只使用y值

兼容问题实在是太复杂了，因此封装为一个方法

```javascript
/**
 * 获取滚动的头部距离和左边距离
 * scroll().top scroll().left
 * @returns {*}
 */
function scroll() {
    if(window.pageYOffset !== null){
        return {
            top: window.pageYOffset,
            left: window.pageXOffset
        }
    }else if(document.compatMode === "CSS1Compat"){ // W3C
        return {
            top: document.documentElement.scrollTop,
            left: document.documentElement.scrollLeft
        }
    }

    return {
        top: document.body.scrollTop,
        left: document.body.scrollLeft
    }
}
```

### 获取可视区域的宽高

**client**

clientWidth clientHeight

网页的可见区域宽高

document.body.clientWidth

clientLeft clientTop

返回的是元素边框的Boderwidth

如果不指定边框，返回0

获取网页的可视区域

兼容处理

```javascript
/**
 * 获取屏幕的宽度和高度
 * @returns {*}
 */
function client() {
    if(window.innerWidth){ // ie9+ 最新的浏览器
        return {
            width: window.innerWidth,
            height: window.innerHeight
        }
    }else if(document.compatMode === "CSS1Compat"){ // W3C
        return {
            width: document.documentElement.clientWidth,
            height: document.documentElement.clientHeight
        }
    }

    return {
        width: document.body.clientWidth,
        height: document.body.clientHeight
    }
}
```

#### onresize

当窗口或框架的大小发生改变的时候会被调用

调用：window.onresize=function(){}

结合获得网页的宽高使用 

### 获取自身的宽高和位置

offsetWidth和offsetHeight

获取对象自身的宽度和高度，包括内容，边框，内边距

offsetLeft和offsetTop

距离第一个**有定位**的父级盒子的左边和上边的距离。可以追踪到body

从父元素的内padding开始计算，到子元素的border外

offsetParent

获得当前有定位的父元素

offsetXXX和style.XXX的区别

a) style.left只能获取行内的，而offsetLeft则可以获取到所有的；
b) offsetLeft 可以返回没有定位盒子距离左侧的位置；而style.left不可以，其只能返回有定位盒子的left;
c) offsetLeft 返回的是数字，而 style.left 返回的是字符串，除了数字外还带有单位：px;
     注意：可以用parseInt进行转化；比如：styleLeft='300px' ---> parseInt(styleLft) ---> 300
d) offsetLeft是只读的，而style.left是可读写；
e) 如果没有给 当前 元素指定过 top 样式，则 style.top 返回的是空字符串。

