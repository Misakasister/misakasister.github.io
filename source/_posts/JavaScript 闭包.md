---
title: JavaScript 闭包
tags:
- 前端
- JavaScript
categories: 编程
---
我研究了两天，终于把闭包啃下来了...

## 函数 声明 表达式

首先我们要区分下函数的声明与函数表达式

```js
声明：
function test (){}

表达式：
var test = function() abc(){} //test.name=abc

var demo = function() {}//匿名函数 demo.name=demo
```
这两个概念理解清楚会在后面经常出现，注意进行区分，函数声明与函数表达式不一样。

## 预编译

理解JS代码的执行顺序：首先会扫一遍代码有没有语法错误，有错误的话就报错就不执行了，没错误就会进行预编译。

首先说明函数的预编译，然后再说明全局的预编译。说明的顺序与执行的顺序是相反的，两者很相似。

### 函数 预编译

首先需要区分的是变量声明与赋值

`var a=100`

`var a` 是声明 `a=100` 是赋值，注意区分

**预编译发生在函数执行的前一刻**

分为4步：

```txt
1 创建AO对象（Activation Object) 也就是作用域 专业名称叫做执行期上下文

2 找形参和变量声明，将变量和形参名作为AO属性名，值为undefined

3 将实参值和形参值统一

4 在函数体里面找函数声明 （函数表达式不行），值赋予函数体
```

接下来我们举个例子来理解这4步

```javascript
function fn(a) {
            console.log(a);
            var a = 123;
            console.log(a);
            function a() { }
            console.log(a);
            var b = function () { }
            console.log(b);
            function d() { }
        }

        fn(1);
```

试问控制台的输出。我们按着预编译的步骤进行分析

在系统刚要执行fn(1)时

1  创建AO对象（Activation Object)

系统内部创建了一个AO对象

```txt
AO{
    
}
```

2 找形参和变量声明，将变量和形参名作为AO属性名，值为undefined

函数的形参为a 变量声明有 a b, 因此 

```txt
AO{
    a:undefined,
    b:undefined
}
```

3 将实参值和形参值统一

```txt
AO{
    a:1,
    b:undefined
}
```

4 在函数体里面找函数声明 （函数表达式不行），值赋予函数体

```txt
AO{
    a: function a() { },
    b: undefined,
    d：function d() { }
}
```

到此，预编译过程完毕了。

接下来开始运行fn(1)

AO相当于一个仓库，变量的值就从里面拿

所以第一个控制台输出function a() { }

第二个控制台输出时a被赋值为123，因此输出123

第三个同上

第四个b被赋值了一个函数，因此输出函数。

![console](http://m.qpic.cn/psb?/V13oJHnV2YpCgI/LuMK6SWgdlmyRMZWa.dC1YCJvnkDodTIt5VUkSeCtnY!/b/dFkAAAAAAAAA&bo=uwBsAAAAAAARB.c!&rf=viewer_4)

### 全局 预编译

全局预编译与函数预编译的主要不同是**全局预编译没有形参，产生GO对象**

因此只有三步：

```txt
1 创建GO对象（Global Object) GO对象就是window对象

2 找变量声明，将变量和形参名作为GO属性名，值为undefined

3 在全局里面找函数声明 ，值赋予函数体
```

系统的全局预编译要比函数预编译先，可以看下面的例子：

```javascript
  global =100;
        function fn(){
            console.log(global);
            global = 200;
            console.log(global);
            var global = 300;
        }

        fn();
        var global;
```

还是说出控制台的输出

1 类似的，系统产生的GO对象为

```TXT
GO{
    global:undefined,
    fn:function fn(){}
}
```

2 当fn执行前期有

```txt
GO{
    global:100,
    fn:function fn(){}
}

AO{
    global:undefined
}
```

fn开始执行。

因此第一个输出undefined，

第二个输出200.

![console](http://m.qpic.cn/psb?/V13oJHnV2YpCgI/DKTWSlYvnoEYqaBjN2wtsq1FUe8Jmv9pmT8EGyZpOR0!/b/dIMAAAAAAAAA&bo=eQBBAAAAAAARFxg!&rf=viewer_4)

### 补充

上述操作可以简单的理解为

```txt
函数声明整体提升

变量 声明提升
```

这也是函数体写在函数调用下面可以运行的原因

另外

1 imply global 暗示全局变量：即任何变量，如果变量未经声明就赋值，此变量为全局对象所有。（window）

```js
a=10;-->window.a

function test(){
    var b=c=123;
}
//AO识别不了c变量，因为c没有进行声明，直接进行赋值
//c被GO识别
console.log(c);//window.c->123
console.log(b);//undefined
```

2 一切声明的**全局**变量，都是window的属性

```js
var b=11;-->window.b
```
## 作用域链

根据我们以前的经验，函数是可以访问外部变量的，这就与作用域有关

```txt 
[[scope]]:每个JS函数都是一个对象，对象中有些属性我们可以访问，但是有些不可以，这些属性仅供JS引擎存取，[[scope]]就是其中一个。[[scope]]指的就是我们所说的作用域，其中存取了运行期上下文的集合。

作用域链：[[scope]]中所存储的执行期上下文对象的集合，这个集合呈链式链接，我们把这种链式链接叫做作用域链。
```

接下来我们进行解释：

前面我们知道了JS文件运行前系统创建GO对象，函数运行前创建AO对象。

产生的对象会放在作用域链的首位。

比如：

```javascript
        function a(){
            function b(){
            }
            b();
        }
        a();
```

a**执行时**，会产生作用域链，如图：

![console](http://m.qpic.cn/psb?/V13oJHnV2YpCgI/.pim.4uywE1bIutKDeH.P9ve2f.5i4vnlONAIPFZzZI!/b/dDQBAAAAAAAA&bo=IQN*AQAAAAARF3w!&rf=viewer_4)

作用域链的第一位是a的AO，第二位是GO

当b函数执行时，**会在a的基础上增加自己的AO**

![console](http://m.qpic.cn/psb?/V13oJHnV2YpCgI/671eCRv6RhZqLIL9mykdP6K1s5nhs2sTOytpg*TQ1bY!/b/dDYBAAAAAAAA&bo=UQPhAQAAAAARF5I!&rf=viewer_4)

b作用域链的第一位是自己产生的AO，后两位与a的作用域链一样。（类似于站在巨人的肩膀上那样...)

两个链指向的a AO是同一个，如果b将某一变量值改变，在a里面调用时，变量值也会变。

scope chain 就是作用域链，js执行时会在链的首端找变量，如果AO里没有该变量的话就会依次往后寻找。

当a的函数执行完毕后，会剪断作用域链与a AO的联系。但是a AO里存着b函数，现在，a AO失去了联系，也就是可以说b函数可以说是”消失“了。

下次a函数执行时，会产生一个新的a AO。

观察这个例子，控制台会输出什么。

```javascript
        function a(){
            var num=0;
            function b(){
                num++;
                console.log(num);
            }
            b();
        }
       a();
       a();
```

这个很简单，会输出1 1

下面这个呢

```javascript
        function a(){
            var num=0;
            function b(){
                num++;
                console.log(num);
            }
            return b;
        }
        var t=a();
        t();
        t();
```

答案是 1 2

其也可以作用域链解释

在var t=a()执行后，a函数已经执行完毕，剪断了a AO，然后t()开始执行。如图

![console](http://m.qpic.cn/psb?/V13oJHnV2YpCgI/EYm1EU7IcGBH0cjSw8zzdULR.C08vU6520TWE*2NuqE!/b/dFYBAAAAAAAA&bo=SgPcAQAAAAARF7Q!&rf=viewer_4)

输出1

虽然a函数a AO失去了联系，但是**b函数还连着a AO**,b执行完毕后，剪断与b AO的联系。这样的话，a的AO与b函数就绑定了。

第二次调用时，会在上图的基础上num=1时加1，因此输出2。

这两个例子的不同就是第二个例子在b函数被销毁前（也就是a函数与a AO剪断关系前）将b函数当作返回值返回到了全局中。

这就涉及到了本篇文章的主角--闭包

## 闭包

当内部函数被保存到外部时，将会生产闭包。闭包会导致原有作用域链不释放，造成内存泄漏。

先说明一下内存泄漏，上面那个例子，本来a函数执行完，应该剪断与a AO的联系，但是b函数却保存下来，如果函数嵌套的比较深，就会造成函数的作用域链很长，被电脑保存起来，这就时内存泄漏（反向理解）。

闭包可以简单理解为内部函数保存了外部函数的劳动。

接下来是最常遇见的闭包情况：

```javascript
        function test(){
            var arr = [];
            for(var i = 0;i < 10;i++){
                arr[i]=function(){
                    console.log(i);
                }
            }
            return arr;
        }
        var t=test();
        for(var j =0; j< 10;j++){
            t[j]();
        }
```

控制台会输出10个10,而并不是0，1，2，....9。

**需要注意的是，arr[i]=function(){}，赋值时，函数体并不执行。**只有当调用时，函数才会执行（函数名后面跟着个小括号），如果改成这样`arr[i]=function(){}()` 这是函数就会立即执行了，输出的也是0 ，1，2，....9了，当然，返回值数组里放的就不是函数体了。（会报错，但是会输出）

![console](http://m.qpic.cn/psb?/V13oJHnV2YpCgI/kVKtYmvbkEjlx.krlhtKB9jPnaSLhmEF7Fdos2qLXmk!/b/dFYBAAAAAAAA&bo=aAE1AQAAAAARF30!&rf=viewer_4)

在t=test()时，test()运行完毕，将10个函数存在arr里面

也就是说t[j]执行时，arr[j]里保存的函数作用域链有一个位置保存了test AO，此时test AO里面的i=10。arr[j] AO里没有i变量，因此在test AO里那了 i 值，所以输出了10。（10个arr[j]的作用域共同连着一个test AO）

这不是我们想要的结果，我们想让控制台输出0 1 2 3...9,因此就有解决方案。

## 立即执行函数

一般函数被声明后,会一直等待函数被执行。直到js文件运行完毕，才会被销毁。

立即执行函数，顾名思义，就是该函数被读到时会立即执行，执行完毕后被销毁。

形式为:

```txt
(function (){}())；W3C 建议
（function (){})();
```

上面闭包的举的粗略解决方案，就用了类似于立即执行函数，在后面添加`()` (执行符号)，立即执行后，函数名会被销毁。

```javascript
var t = function (){}();
console.log(t)；//undefined
```

只有表达式才能被执行符号执行`()`

```javascript
function a(){}();//不行，这是声明
+function a(){}();//可以，这是表达式
```

知道了立即执行函数可以解决这个问题，因此我们可以把代码改成：

```javascript
     function test() {
            var arr = [];
           for(var i =0; i< 10; i++){
               (function(i){
                   arr[i]=function(){
                       console.log(i);
                   }
               }(i))
           }
            return arr;
        }
        var t = test();
        for (var j = 0; j < 10; j++) {
            t[j]();
        }
```

这样控制台就输出0，1， 2，...9了。

