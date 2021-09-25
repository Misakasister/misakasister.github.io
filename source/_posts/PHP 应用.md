---
title: PHP 应用
tags:
- 后端
- PHP
categories: 编程
---

## 表单传值

表单传值即浏览器通过表单元素将用户的选择或者输入的数据交给后台服务器

### 传值的方式

#### GET传值

1 from 表单

`<form method="GET"  action="url"> 表单元素</form>`

2 a标签

`<a href="../index.php?data="php">`

3 location 对象的href 属性

`location.href=""`

4 location 对象的assign()方法

`location.assign("...")`

提交要放到html结构的最后面

#### post 传值

1 post表单传值

`<form method="POST"> 表单元素</form>`

2 post与get的区别

get主要用来获取数据，不改变服务器上的资源，get只是用来获取内容

post传输的数据主要用来增加数据，改变服务器上的资源：post会改变服务器上数据内容

3 post必须使用form表单，get可以使用form和url

4 get 传输的数据在url中可见，post不可见，get传值显示

`?数据名=数据值&数据名=数据值`

5 get能传2k（浏览器限制,并非本身的属性），post理论上无限制

6get 传递简单的数据（数值/字符串）post传递复杂数据（二进制等）。

### 接收数据

$_GET 接受get方式提交的数据

$_POST 接收post方式提交的数据

$_REQUEST 接收post方式或者get方式提交的数据

这三个方法都是php超全局，没有范围的限制，预定义数组，表单元素的“name"属性的值作为数组的下标，value属性对应的值就是数组元素的值。点击提交后，$_...就会接受表单的值。

**因此表单必须要有name值作为角标，value值作为数组值**

```html
<form action="echo.php" method="GET">
        <input type="text" name="text">
        <input type="submit" name="sub">
    </form>
```

```php
<?php
 $a = $_GET;
 var_dump($a);
?>
```

![get](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/SJ2jyOP6507RuZlJeBSXhHyVAFtXstN7Mra..LTm.*8!/b/dDYBAAAAAAAA&bo=0QKwANECsAADGTw!&rf=viewer_4)

#### 复选框处理

单选框的处理:radio

可以出现多个选项，但是只能选择一个

表单使用name 属性，使用同名即可：只能选择一个

```html
<input type="radio" name="gender" value="1">男
<input type="radio" name="gender" value="2">女
```

php拿到数据后，组织sql直接存到数据表里即可

多选框的处理：

1表单中name 属性使用数组格式:`名字[]`    **name值必须加中括号**  //一类复选框数据使用一个

```html
<input type="checkbox" name="hobby[]" value="1">足球
<input type="checkbox" name="hobby[]" value="2">蓝球
<input type="checkbox" name="hobby[]" value="3">排球
<input type="checkbox" name="hobby[]" value="4">球
```

```php
<?php
 $a=$_GET[hobby]；//数组a就接受了被选选项的value值   
 ?>
```

2后台接收数据之后，是一个数组（数组不能存储到数据库中）

![get](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/7F8SNiQRT56OLjAgHoeAkOJ.VpUt2Hae0gj1RaE4Huo!/b/dDYBAAAAAAAA&bo=JAF9ACQBfQADGTw!&rf=viewer_4)

3php 需要使用分隔符讲数组分解为字符串

implode(分割符，数组)

反向操作：expload 可以把字符串变成数组

## 文件上传处理

表单写法

1) method 的提交方式必须为Post

2）enctype 属性  form表单属性，主要是规范表单数据的编码方式。

http://www.w3school.com.cn/tags/att_form_enctype.asp

属性值

| 值                                | 描述                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| application/x-www-form-urlencoded | 在发送前编码所有字符（默认）                                 |
| multipart/form-data               | 不对字符编码。在使用包含文件上传控件的表单时，必须使用该值。 |
| text/plain                        | 空格转换为 "+" 加号，但不对特殊字符编码。                    |

3）上传表单：file表单

```html
 <form action="echo.php" method="POSTenctype="multipart/form-data">
 <input type="file" value="img" name="loimg">
 <input type="submit" value="true" name="t">
 </form>
```

### $_FILES

预定义变量$_FILES 是专门用来存储用户上传文件的

```php
<?php
 $a = $_POST;
 $b = $_FILES;
 var_dump($a);
 var_dump($b);
?>
```

![$_files](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/*I0zZwRXqy9WnADW4RqJ0iO0RosAqqFBw13ZIplxVCk!/b/dDUBAAAAAAAA&bo=LQLNAC0CzQADGTw!&rf=viewer_4)

上图是b数组的内容，可以看到角标name=img对应的元素为一个数组，其数组内容又包含：

name: 文件在客户机上的实际名字 （实际用来保存后缀）

type:MIME(多功能互联网邮件扩展)类型，用来在计算机中识别文件类型，比如识别到doc就可以用word打开

tmp_name: 文件在服务器端临时存储的地址和名字（等待脚本被加工，不处理则被删除。）

error:文件上传的代号，用来告知php文件接受过程中出现了什么错误

size: 文件大小

### 移动临时文件

1）判断是否为上传的文件 is_uploaded_file() 判断文件是否为http post 上传，参数是tmp_name

2）移动文件：move_uploaded_file()

参数1：源文件在哪里（tep_name)

参数2：想要保存的url，url需要有文件的新名字（name)

文件上传后会保存到$_file中，那么访问文件信息的形式就是\$\_file["name值"\]["属性值"]

```php
<?php
 $a = $_FILES['loimg'];
 if (is_uploaded_file($a['tmp_name'])) {
     move_uploaded_file($a['tmp_name'], '..\\'.$a['name']);\\第一个反斜杠是转义的意思 或者写错../也行
 }
```

### 多文件上传

#### 结构

##### 同名表单

```html
 <form action="echo.php" method="POST" enctype="multipart/form-data">
<input type="file" name="hobby[]" value="1">
<input type="file" name="hobby[]" value="2">
<input type="file" name="hobby[]" value="3">
<input type="file" name="hobby[]" value="4">
<input type="submit">   
</form>
```



会将表单名形成一个数组，而且将文件对应的五元素（name, tmp_name等）再形成一个数组，每个文件对应元素下标所对应的文件是一样的，name[0]与tmp_name[0]对应是用一个文件

![$_files](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/ROKVA6P1XHd8h*aR*7tLZT8x60FkqTQd3BWGHX2Hte8!/b/dC8BAAAAAAAA&bo=SQKEAkkChAIDGTw!&rf=viewer_4)

##### 不同名表单

每个文件都会形参一个下标为name的数组

```html
<form action="echo.php" method="POST" enctype="multipart/form-data">
<input type="file" name="h1" value="1">
<input type="file" name="h2" value="2">
<input type="file" name="h3" value="3">
<input type="file" name="h4" value="4">
<input type="submit">  
```

![$_files](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/NKouTIHxeWRMTTqtjg73WhqZSW8X0ubRGXgSM8hTyj4!/b/dFIBAAAAAAAA&bo=QgI0AkICNAIDGTw!&rf=viewer_4)

#### 多文件的遍历与读取

不同名文件直接$files[name]就可以取出来

如果文件的个数不确定，就要用到foreach去遍历

```php
<?php
    foreach($FILES as $key){
        //$key就是一个完整的上传文件信息
        if(is_uploaded_file($key['tmp_name'])){
            move_uploaded_file($key['tmp_name'],'url');
        }
    }
```

 同名多文件上传：想办法得到一个文件对应的五个元素。从$_FILES中把对应元素取出来，之后存放在一个数组中。

```php
<?php
    if(isset($_FILES['form-name']['name'])&&is_array($_FILES['form-name']['name'])){
       $arr=array();
    foreach($FILES['form-name']['name'] as $key => $value){
    	 $arr[]=array(
         'name'=>$value,
          'tmp_name'=>$_FILES['form-name']['tmp_name'][$key],
         )
    }
    }
```

### 文件处理

当上传一个文件，我们需要判断

文件类型是否合适（mime类型）？文件存储到的位置？文件格式的限制（后缀名）？大小的限制？还需要将文件的路径和文件名字返回。

如果上传失败的时候还要告诉用户的错误原因

```php
<?php

// 文件上传（单文件）

/*
array $file 需要上传的文件信息，一位五元数组
array $allow_type 允许上传的mime类型
string $path 存储的路径
string &$error 出现错误的原因
array $allow_format=array() 允许上传的文件格式
int max_size 允许上传的最大值
*/
function upload_single($file, $allow_type, $path, &$error, $allow_formate, $max_size)
{
    // 判断文件是否有效
    if (!is_array($file) || !isset($file['error'])) {
        //文件无效
        $error = '文件无效';

        return false;
    }
    // 判断文件存储路径是否有效
    // is_dir() 函数检查指定的文件是否是一个目录。
    if (!is_dir($path)) {
        $error = '路径错误';

        return false;
    }
    // 判断文件是否上传过程中出错
    // 值为0的时候代表上传成功，而且没有5
    switch ($file['error']) {
        case 1:
        case 2:
        $error = '文件超过服务器允许的大小';

        return false;
        case 3:
        $error = '文件只上传了一部分，检查网络';

        return false;
        case 4:
        $error = '没有选中文件';

        return false;
        case 6:
        case 7:
        $error = '文件保存失败';

        return false;
    }

    // 文件类型处理
    //in_array()判断一个元素是否在数组里存在
    if (!in_array($file['type'], $allow_type)) {
        $error = '当前文件不允许上传';

        return false;
    }

    // 判断后缀名
    // 取出后缀名
    // strstr — 查找字符串的首次出现 stringstrip
    // ltrim从字符串左侧移除字符：
    $ext = ltrim(strstr($file['name'], '.'), '.');
    if (!empty($allow_formate) && !in_array($ext, $allow_formate)) {
        $error = '当前文件不允许上传';

        return false;
    }

    // 判断文件大小
    if ($file['size'] > $max_size) {
        $error = '文件超出大小,最大可传'.$max_size.'字节';

        return false;
    }
    // 构造文件的名字
    $fullname = strstr($file['type'], '/', true).date('Y - m - d');
    for ($i = 0; $i < 4; ++$i) {
        //char(ascii))
        $fullname .= chr(mt_rand(65, 90));
    }
    $fullname .= '.'.$ext;
    // 移动到指定目录
    if (!is_uploaded_file($file['tmp_name'])) {
        $error = '上传失败';

        return false;
    }
    echo $fullname;
    if (move_uploaded_file($file['tmp_name'], $path.$fullname)) {
        return true;
    } else {
        $error = '上传失败';

        return false;
    }
}

$file = $_FILES['h1'];
$path = '../';
$allow_type = array('image/png');
$allow_format = array('png', 'jpg');
if (upload_single($file, $allow_type, $path, $error, $allow_format, 200000)) {
    echo 'yes';
} else {
    echo $error;
}
```

## MySql 扩展

php 针对 MySql 数据库提供的扩展，允许php当作mysql的一个客户端连接服务器进行操作。

### 连接数据库

建立连接：

主机地址，默认自动连接3306

`$link=mysqli_connect('服务器地址'，'用户名'，'密码')`

连接资源默认超全局，任何地方都可以调用

设置连接编码

`mysqli_query($link,'set names utf8')`

`mysqli_set_charset('字符集')`

确定编码：客户端当前所用脚本界面的字符集

### 执行SQL指令

`mysqli_query($link,'sql')`

增：`mysqli_query($link,'inset...')`

`insert into 表名[(字段名列表)] values (对应的字段值)`

删：`mysqli_query($link,'delete...')`

`delete from 表名 where 字段属性值=值`

改：`mysqli_query($link,'update...')`

`update 表名 set 字段属性值 = 新值 [where 条件]`

查：`mysqli_query($link,'select...')`

`select 字段名列表 from 表名`

**执行语句时如果变量的字符串有引号，一定要进行替换处理，展示的时候也要在换回来**

也不一定是查询语句，show和desc都可以，只要是将数据展示的语句就可以

查的返回值有true和false 这并不能说明有没有查到数据（都会返回true)，只有语句写错了会返回false。本身是一个结果集，转化为bool用为true

### 解析结果集

**获取结果的行数**

`mysqli_num_rows(结果集)` 可以得到结果集中到底有多少行记录

将一种结果资源（php不能直接使用）,转换成一种php能够解析的数据格式：通过从结果集中（结果集指针：类似与数组指针）按照结果集指针所在位置取出对应的一条记录（一行）,返回一个数组，同时指针下移，直到指针移出结果集。

`mysqli_fetch_assoc()` 获取关联数组，数组的键是字段，值是记录

 `mysqli_fetch_row()` 获取索引数组，键是数字，值是记录

`mysqli_fetch_array()` 获取关联数组或索引数组，也就是一个数据取两次，通过字段或者数字作为下标都可以得到数据。可以通过第二个参数MYSQL_ASSOC,MYSQL_NUM,MYSQL_BOTH来调整

所有的fetch语句共用一个数据源指针

**一些其它函数**

`mysqli_num_fields($res)` 获取结果集中的字段数

`mysqli_field_name($res,0)` 获取结果集中指定位置的字段

### 关闭资源

主动释放连接,脚本执行结束也会自动释放

`mysql_close('资源')`

## http协议

### 概念

HTTP协议：即超文本传输协议（Hypertext transfer protocol)。是一种详细规定浏览器和万维网服务器之间互相通信的规则，通过因特网传送万维网文档的数据传输协议。

http协议是用于www服务器传输超文本到本地浏览器的传送协议。它可以是浏览器更加高效，使网络传输减少。它不仅保证计算机正确快速地传输超文本文档，还确定传输文档的拿一部分，以及哪部分首先显示等。

### 特点

客户/服务器模式

简单快速：只需传送请求方式和路径

灵活：允许传送任意类型的数据对象（mime类型）

无连接：每次连接只处理一个请求，服务器处理完客户的请求，并收到客户的应答后，即断开连接

无状态：对于事务处理没有记忆能力。如果后续处理需要前面的信息必须重传。

### 分类

请求协议：浏览器向服务器发起请求的时候需要遵循的协议。

响应协议：服务器向浏览器发起响应的时候需要遵循的协议。

### http请求

开发者工具->network中查看

![get](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/1kJKlZAxjvnyhHTchPKOR0PrvI4lL3l9FVH*EuLmNtg!/b/dDEBAAAAAAAA&bo=WwRBAVsEQQEDGTw!&rf=viewer_4)

请求行

形式：请求方式 资源路径  协议版本号

`GET/index.php http/1.1`

请求头：

各项协议内容：具体的协议内容不会每次都使用全部

host：请求的主机地址（必须）

accept: 当前请求能够接受服务器返回的类型（mime)

accept-language: 接收内容

user-agent:客户浏览器所在点的信息

请求体

请求数据：只有post请求会有请求体。get请求所有的数据都是跟在url之后，会在请求行中的资源路径上体现。

### http响应

![get](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/nXR0zBo17w0aEKlwWpC9YLoBOkIrxyGyLu2XoheUOBo!/b/dDYBAAAAAAAA&bo=QQIUAUECFAEDGTw!&rf=viewer_4)

响应行

形式：协议版本 状态码 状态消息（独占一行）

`http/1.1 200 ok`

200 ok 成功

403 forbidden 没有权限访问

404 not found 未找到页面

500 server internal error 服务器内部错误

响应头

- 时间
- 服务器
- 内容长度
- 内容类型

### 常见http响应设置

php针对http协议（响应）进行了底层设计，可以通过函数header来实现修改响应头。

header可以设计http响应，因为http协议特点是：响应行，响应头（空行结尾）响应体。认为通过header设计响应头的时候，不应该有任何内容的输出，所以一旦产生内容输出，系统都会认为响应头结束了，响应体开始了，所以如果先输出内容后设置响应头（header使用），理论设置无效。

在php5以后，增加程序内容缓存内容：允许通过服务器在输出内容的时候，不直接返回浏览器而是先在服务器端使用程序缓存保留，有了该内容后，在程序缓存内会自动调整响应头和响应体（允许响应头在已经输出的内容之后在设置），但是会进行警告。

**因此header设置响应体之前不要有任何输出**

location 重定向 立即跳转（响应体不用解析）

浏览器在解析服务器响应的时候，先判定响应行，继续响应头，最后响应体，location在响应头中，所以浏览器一旦看见该协议便不再向下解析。

refresh 延迟重定向 定时跳转（响应体会解析）

content-type 内容类型 mime类型

通过内容告知（mime)，浏览器正确解析内容

content-disposition 内容类型，mime类型扩展，激活浏览器文件下载对话框，浏览器在解析内容的时候，默认是直接解析的，那么有时候需要浏览器不解析，当作内容下载成文件。

```php
<?php
    header('content-disposition:attachment;filename=名字');
```

### php模拟http请求

浏览器发送http请求向我们的服务器，而我们可以利用php脚本(curl)在我们的服务器里模拟http请求去访问另外一台服务器(也可以自我访问服务器)

原理：

curl是一个开源库，支持很多协议，包含http,ftp等。

前提条件：http协议的客户端、服务器模式，http协议不一定局限于浏览器访问，自己访问自己。

curl 扩展库使用：

建立连接 `curl_init()` 激活curl 连接功能

设置请求选项 `curl_setopt()` 设定选项

http://php.net/manual/zh/function.curl-setopt.php

执行请求 `curl_exec()` 执行选项（与服务器发送请求），得到服务器的返回内容

关闭连接 `curl_close()` 关闭资源

```php
<?php
    $ch=curl_init()//建立资源
    //链接选项
    curl_setopt($ch,CURLOPT_URL.'127.0.0.1/index.php')//连接选项 访问另外一台服务器
    curl_setopt($ch,CURLOPT_RETURNTRANSFER,TRUE);//文件流形式返回
	$c=curl_exec($ch);//执行
    curl_close($ch);//关闭
```

## 文件编程

文件编程指利用php对文件（文件夹）进行增删改查的操作。

### 目录操作

创建目录结构(文件夹)

`mkdir(url)` 创建成功返回true,失败返回false。

错误忽视符号：`@`

 如果文件夹已经存在了，`mkDir(url)`  会报错，但是我们不想要他报错（只需要文件夹存在）那么 `@mkDir(url)` 就可以了

删除

`rmdir(url)`

读取

将文件夹（路径）按资源方式打开

1 `opendir()` 打开资源，返回一个路径资源，包含指定目录下的所有文件（文件夹）

2 `readdir()` 从资源中读取指针所在位置的文件名字，然后指针下移，知道指针移出资源

关闭资源：`closedir()` 

![get](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/Hc3BGpMgNnSD79PQT5rRZn01Xh4UJLtnE70NTc.IBuM!/b/dFIBAAAAAAAA&bo=fgHsAH4B7AADGTw!&rf=viewer_4)

任何目录都包含`.` 自己的目录 和 `..` 上级目录

读取所有内容：

```php
<?php
    $arr=array();
    $r=opendir(url);
	while($f=readdir($r)){//类似于数据库的操作，全部拿出来
        $arr[]=$f;
    }
	var_dump($arr);
	closedir($r);
```

**其它函数**

`dirname(url)` 得到上一层目录,就是把文件夹名或者文件名在url中去掉

`realpath(url)` 得到真实路径（目录路径），如果是文件那么得到false

`is_dir(url)` 判断是否为路径

`scandir(url)` 获取一个指定路径下的所有文件，以数组的形式返回，相当于opendir/readdir/closedir

#### 递归遍历目录

指定一个目录，将其下的所有文件和目录，及目录下的所有内容输出。

设计思路：

设计一个能够遍历一层文件的函数

如果遍历到的文件是目录，应该调用当前函数，注意`.` 和 `..`

递归出口：该目录没有任何子文件夹。

```php
<?php
        $dir = '../';
    function my_scandir($dir, $lv = 0)
    {
        // 不是路径
        if (!is_dir($dir)) {
            dir($dir.'<br/>');
        }
        // 遍历目录
        $file = scandir($dir);
        foreach ($file as $value) {
            // 排除'.' 和'..
            if ($value == '.' || $value == '..') {
                continue;
            }
            echo $value.'<br>';
            for ($i = 0; $i < $lv * 4; ++$i) {
                echo '&nbsp';
            }
            $file_dir = $dir.'/'.$value; //构造目录，$value是一个文件名字
            if (is_dir($file_dir)) {
                my_scandir($file_dir, ++$lv); //一定要传路径
            }
        }
    }
    my_scandir($dir);
```

### 文件操作

php5 常见文件操作函数

1） `file_get_contents(url)` 获取指定文件的所有内容， 返回布尔值

2） `file_put_contents(url，content)` 将指定内容写入指定文件内 返回的结果是字符串长度（字节）如果当前路径下不存在该文件，该函数会自动创建，但是目录不存在不行。

php4 文件操作函数

php4 中是将文件操作用资源的形式处理，不论是读还是写都依赖于资源指针。

1）`fopen(url, 打开模式)`  打开一个文件资源，限定打开模式

http://www.php.net/manual/zh/function.fopen.php

 2）`fread($res,长度)` 从打开的资源读取一定长度

3）`fclose(url)` 关闭资源

其它文件操作：

1) `is_file()` 判断文件，不识别路径

2）`filesize()` 获取文件大小

3）`file_exists()` 判断文件，识别路径

4）`unlink()` 取消文件名字与磁盘地址的链接（删除文件）

5）`filemtime()` 获取文件最后一次修改时间

6 ）`fseek()` 设定fopen打开的文件指针位置

#### 文件下载

从服务器将文件通过http协议传输到浏览器，浏览器不解析保存成相应的文件。

提供下载方式可以通过a标签， `<a href="互联网绝对路径"></a>` 但是a标签只能下载无法解析的文件（比如不能下载.html)，而且这样会暴露服务器存储数据的位置

通过php下载

设定响应头：

文件返回类型 image/jpg || application/octet-stream （以流的形式下载文件,这样可以实现任意格式的文件下载。）

文件的计算方式：accept-ranges:bytes

设定下载提示：content-disposition:attachment;filename='文件名字'

设定文件大小：accept-length：

## 会话技术

用户打开一个浏览器，访问某个web站点，在这个站点点击多个超链接，访问多个web资源，整个过程称之为一个会话。

http协议的特点是无状态/无连接，当一个浏览器连续多次访问一个服务器时，服务器是无法区分多个操作系统是否来自同一个浏览器(用户)。会话技术就是通过http协议想办法让服务器能够识别来自同一个浏览器的多次请求，从而方便浏览器（用户）在访问同一个网站的多次操作中，能够持续进行而不需要进行额外的身份验证。

### 分类

cookie技术

是在http协议下，服务器或脚本可以维护客户工作站上信息的一种方式。**cookie 是由web服务器保存在用户浏览器（客户端）上的小文本文件**（http响应头），他可以包含有关用户的信息，无论何时用户链接到服务器,站点都可以访问cookie信息。

session技术

时域：指一个终端用户与交互系统进行通信的时间间隔，通常指从注册进入系统到注销退出系统之间经过的时间，以及如果需要的话，可能还有一定的操作空间，**session技术是将数据保存到服务器端,**无论用户何时连接到服务器，站点都可以访问session信息。

session技术的实现依赖于cookie技术

区别：

| --       | cookie                      | session                |
| -------- | --------------------------- | ---------------------- |
| 安全性   | 存储浏览器端，安全性低      | 存储服务器端，安全性高 |
| 数据大小 | 数量大小都有限制（20个/4k） | 不限                   |
| 数据类型 | 简单数据，数值 字符串       | 复杂数据 自动序列化    |
| 保存位置 | 浏览器                      | 服务器                 |

### cookie

服务器将数据通过http响应存储到浏览器上,浏览器可以在以后携带对应的cookie数据访问服务器。

![get](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/OJhJbQfjcPyoknbxHHN6L7MIr3YaOQsG3bdWwvskTdY!/b/dFMBAAAAAAAA&bo=bAWAAgAGxAIDKSc!&rf=viewer_4)

1 第一次请求时，php通过setcookie 函数将数据通过http协议响应头传输浏览器

2 浏览器在第一次响应时将cookie数据保存到浏览器

3 浏览器后续请求同一个网站的时候，会自动检测是否存在cookie数据，如果存在将在请求头中将数据携带到服务器。

4 php 执行的时候会自动判断浏览器是否携带cookie，如果携带，就自动保存到$_COOKIE中

5 利用$_COOKIE访问Cookie数据

#### 基本使用

创建cookie

`setcookie(名字，值)` 名字为字符串 ，值必须时简单类型字符串或者整数

具体的cookie内容在谷歌浏览器中的设置->内容设置->cookie中可以查看

访问cookie值

`$_COOKIE`  预定义变量 得到的是数组

**cookie可以实现跨脚本共享数据**

#### 生命周期

cookie在浏览器生存时间（浏览器在下次访问服务器的时候是否携带cookie）

默认（不设定）时的生命周期：关闭浏览器（会话结束）

设定一个常规时间戳的周期，通过setcookie的第三个参数确定，**setcookie('a','a',time()+时间)，一定要加时间戳。**

设定为0的周期，第三个参数如果为0的话就是默认设置

删除一个cookie的做法：服务器没有权限没有去操作浏览器上的内容，可以通过设定生命周期让浏览器自动判定cookie是否有效，无效就清除

```php
<?php
    setcookie('a1','');//清空内容
    setcookie('a2','a2',time());//过期
```

#### 作用范围

默认（不设定）的范围

不同的文件夹层级中，设定的cookie默认是在不同的文件夹下有访问限制。上层文件夹中设定的cookie可以在下层（子文件夹）中访问，而子文件夹中设定的cookie不能在上层文件夹中访问。

设定为`/` 告知浏览器当前的cookie的作用范围时网站根目录,setcookie的第四个参数

跨子域

在同一级别域名下，可以有多个子域名

不同的域名（主机）之间不能共享cookie

可以通过setcookie的第五个参数控制--有效域名

在设定域名访问的时候设定上级域名即可：we.com 所有以we.com结尾的域名都可以访问该cookie

#### 数组的使用

设置形式`setcookie('c[k1]','value')` 

读取形式 `$_COOKIE['c'][k1] ` 

### session

![get](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/LmnblqQqW1EJU84zecgiwAPJBByRLID7SE.dk8xqB64!/b/dDUBAAAAAAAA&bo=YwWAAl0G9AIDKUU!&rf=viewer_4)

php碰到session_start() 时开启session会话，会自动检测sessionID

如果cookie中存在，使用现成的。

如果cookie不存在，创建一个sessionID，并通过响应头以cookie的形式保存到浏览器上。

初始化超全局变量$_SESSION 为一个空数组

php通过sessionID去指定位置（session文件存储的位置）匹配对应的文件

不存在该文件，创建一个sessionID 命名文件

存在该文件，读取文件内容（反序列化），将数据存储到$_SESSION中

脚本执行结束,将$_SESSION中保存所有数据序列化存储到sessionID 对应的文件中

#### 基本使用

启用session，任何时候都要开启session（脚本使用到$_SESSION就开启一次）

```php
<?php
    session_start();//开启session
	//设置sessions数据
	$_SESSION['name']='mark';
	$_SESSION['hobby']=array("footbal","basktbal");
```

**可以跨脚本访问，实现数据的共享。（两个脚本都要有session_start()）**

删除一个session信息使用`unset()`

删除全部的数据：`$_SESSION=array()`

##### 基础配置

在php.ini中

`session.name` 保存到cookie中sessionID对应的名字

`session.auto_start` 是否自动开启session（无需手动session_start())

`session.save_handler` session数据的保存方式，默认是文件形式

`session.save_path` session文件存储到的位置（需要修改，默认系统的临时文件存储）

`session.cookie_lifetime` sessionID对应cookie的生命周期，默认是会话结束

`session.cookie_path` sessionID在浏览器存储之后允许服务器访问的路径

`session.cookie_domain` 允许访问的子域

全局配置，直接在php.ini中修改

脚本中配置，`ini_set(名称，值)` 函数在运行中设定某些配置项（只会对当前运行的脚本有效）

##### 销毁session

删除session对应的session文件

`session_destroy()` 

##### 垃圾回收机制

给session文件指定生命周期，通过文件最后的修改时间与生命周期进行结合判断，过期了就可以把对应的session文件删除

 ```txt
任何一次session开启（seesion_start)，session都会尝试读取session文件
读取session文夹后，有几率触发垃圾回收机制
垃圾回收机制会自动读取所有的seesion文件的最后编辑时间与生命周期进行判断。
 ```

垃圾回收的配置

`session.gc_maxlifetime= 1440` session文件的最大生命周期是1440秒

`session.gc_probability= 1` 垃圾回收概率分子

`session.gc_divisor=1000` 垃圾回收概率分母

##### 禁用cookie后的使用

session技术的使用需要利用cookie技术来保存sessionID,从而使php能够得到相同的seesionID,从而访问相同的session文件。

没有cookie，脚本执行到 session_start 就会产生一个新的sessionID 和session文件。

解决方案：session_id 和session_name 来获得sessionID 或者 name 从而解决session_start产生新的sessionID

```php
<?php
    session_start();//开启

	//获取sessionID和名字
	$id= session_id();
	$name=session_name();//默认phpsessid
	
	//设置内容
	$_SESSION['NAME']='MARK';

	//传递给另外一个脚本
	echo "<a href = 'tmp.php?{$name}={$id}'>click</a>";
```

另外一个脚本接受

```php
<?php
    $name=session_name();
	$id=$GET['name'];

	//设定sessionID
	session_id($id);

	//访问
	session_start();
```



  







