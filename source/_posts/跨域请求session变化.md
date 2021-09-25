---
title: 解决跨域请求session值变化的问题
tags:
- Java
- 踩坑记录
categories: 编程
---

问题描述：

前端环境：vue-cli 本机8080端口

后端环境： tomcat 本机8079端口

前端ajax请求无法传达后端，浏览器关闭同源策略后，后端的sessionid在不同的控制器变化，提示无法建立会话，并且前端浏览器没有cookie存储，

原理:略。

解决方案：

前端配置

在main.js里设置

```vue
axios.defaults.withCredentials=true,
```

后端配置：

控制器类加：

```spring
@CrossOrigin(origins = "*", maxAge = 3600)
```

maven下下载插件

```maven
<dependency>
    <groupId>org.ebaysf.web</groupId>
    <artifactId>cors-filter</artifactId>
    <version>1.0.1</version>
</dependency>
```

在web.xml配置

```web-idl
<!-- CORS-->
  <filter>   
  <filter-name>CORS</filter-name>   
  <!--类位置一般要改-->
  <filter-class>org.ebaysf.web.cors.CORSFilter</filter-class>
     <init-param>    
       <param-name>cors.allowOrigin</param-name>   
          <param-value>*</param-value>   
          </init-param>   
          <init-param>      
          <param-name>cors.supportedMethods</param-name>     
           <param-value>GET, POST, HEAD, PUT, DELETE</param-value> 
             </init-param>   
             <init-param>     
              <param-name>cors.supportedHeaders</param-name>    
                <param-value>Accept, Origin, X-Requested-With, Content-Type, Last-Modified, Access-Control-Allow-Origin</param-value>   
                </init-param>   
                <init-param>     
                 <param-name>cors.exposedHeaders</param-name>  
                     <param-value>Set-Cookie</param-value>  
                      </init-param>   <init-param>     
                       <param-name>cors.supportsCredentials</param-name>    
                         <param-value>true</param-value>  
                          </init-param></filter>
                          <filter-mapping>   
                          <filter-name>CORS
                          </filter-name>   
                          <url-pattern>/*</url-pattern>
                          </filter-mapping>
```

