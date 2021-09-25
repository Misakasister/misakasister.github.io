---
title: Vue 配合 axios 上传文件（图片）
tags:
- 前端
- JavaScript
- Vue
categories: 编程
---

## 基础起步

上传文件要用到input标签

`<input type="file" />` 

文件选择是只读的,不能使用v-model 数据双向绑定,只能用change监控值的变化。

所以就用change来代替v-model来处理文件。

`<input type="file" v-on:change="upFile" />`

在methods里处理文件

```vue
    methods: {
        upFile(e){
            console.log(e.target.files);
        }
    }
```

e.target.files就是点击上传的文件对象了

![QQ截图20190731190836](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/Yl10JcRV4GivKwMDxNVZZnnlYPJH.qUDp*DLm3jJ8Ns!/b/dLgAAAAAAAAA&bo=yQLeAAAAAAADBzc!&rf=viewer_4)

如果在input标签中加入 multiple 属性，可以在一个输入框中选择多个文件进行上传，此时e.target.files就是一个数组，用下标去寻找文件。

![QQ截图20190731191300](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/g*4Pq7V6YKMVKNcST3pcgCieRxsTq6W0egueNeZzfPw!/b/dMUAAAAAAAAA&bo=agJ6AAAAAAADFyA!&rf=viewer_4)

## 如何在前端预览

我搜索到的方法是将图片转换成base64的编码，之后用`img`的`src`等与图片的base64编码即可展示。

`img` 的 `src` 等于图片的URL可以展示，等于base64也可以展示。

将图片转换成base64需要用到内置对象<a href="<https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader>">**FileReader**</a>

当 **FileReader** 读取文件的方式为  [readAsArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/API/FileReader/readAsArrayBuffer), [readAsBinaryString](https://developer.mozilla.org/en-US/docs/Web/API/FileReader/readAsBinaryString), [readAsDataURL](https://developer.mozilla.org/en-US/docs/Web/API/FileReader/readAsDataURL) 或者 [readAsText](https://developer.mozilla.org/en-US/docs/Web/API/FileReader/readAsText) 的时候，会触发一个 `load` 事件。从而可以使用  **FileReader.onload** 属性对该事件进行处理。

读取图片的base64需要用到`readAsDataURL`这个方法是异步的，也就是说，只有当执行完成后才能够查看到结果，如果直接查看是无结果的，并返回undefined，也就是说必须要挂载 实例下的 onload 或 onloadend 的方法处理转化后的结果。

代码如下：

```html
<input type="file" v-on:change="upFile"/>
<img src ref="showImg">
```

```vue
    methods: {
        upFile(e){
            let reader=new FileReader();
            reader.onload= (evt)=>{
                this.$refs.showImg.src=evt.target.result;
            }
            reader.readAsDataURL(e.target.files[0]);
        }
    }
```

`evt.target.result` 就是`reader.readAsDataURL(e.target.files[0])` 的结果。

效果如下：

![Animation](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/edpOSrz*fw.IMR*XOf4rn41OyCnkLTpxtSObW9SfR8M!/b/dFQBAAAAAAAA&bo=9QMkAgAAAAACR7M!&rf=viewer_4)

## 配合axios 上传至后台

如果直接传base64编码的话，可以按照普通的字符传值处理，存入数据库时可以在后台将base64转换成图片存放在某处，数据库直接存放生成的图片的URL即可。再往前端传图片时可以传base64也可以传URL。

现在说明的是如何将文件直接传到后台（PHP用$_FILES接受）。

<a href="<https://developer.mozilla.org/zh-CN/docs/Web/API/FormData>">**FormData**</a> 接口提供了一种表示表单数据的键值对的构造方式，经过它的数据可以使用 [`XMLHttpRequest.send()`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/send) 方法送出，本接口和此方法都相当简单直接。如果送出时的编码类型被设为 `"multipart/form-data"`，它会使用和表单一样的格式。

代码解释如下

```vue
upFile(e) {
      let file = e.target.files[0];
      let param = new FormData(); // 创建form对象
      param.append("file", file, file.name); // 通过append向form对象添加数据
      param.append("chunk", "0"); // 添加form表单中其他数据
      console.log(param.get("file")); // FormData私有类对象，访问不到，可以通过get判断值是否传进去
      let config = {
        headers: { "Content-Type": "multipart/form-data" }
      };
      this.axios
        .post(apiURL, param, config)
        .then(function(response) {
          console.log(response);
        });
  }
```

后台即可用\$_FILES接受。其它值用\$_POST 接受。

![QQ截图20190731200544](http://m.qpic.cn/psb?/V13oJHnV4UEUs2/icrnc*PzoB3Mt2fQD*OjeQRNsygViM6Za50P*YRhnoQ!/b/dLgAAAAAAAAA&bo=KQL*AAAAAAADF.Y!&rf=viewer_4)

后台如何处理文件，参考 https://misakasister.github.io/2018/09/30/PHP%20%E5%BA%94%E7%94%A8/ 中的文件上传部分。

## 全部代码

```html
<div id="app">
      <input type="file" v-on:change="upFile"/>
      <img src ref="showImg" />
</div>
```

```vue
 upFile(e) {
        //图片预览部分
        let reader = new FileReader();
        let file = e.target.files[0];
        reader.onload = evt => {
          this.$refs.showImg.src = evt.target.result;
        };
        reader.readAsDataURL(file);
        //图片上传部分，可以用公共变量保存param，在别的事件里上传。
        let param = new FormData();
        param.append("file", file, file.name);
        param.append("chunk", "0");
        let config = {
          headers: { "Content-Type": "multipart/form-data" }
        };
        axios
          .post(
            " http://localhost:8088/sql/CodeIgniter-3.1.10/index.php/file/upload",
            param,
            config
          )
          .then(function(response) {
            console.log(response);
          });
      }
    }
```

