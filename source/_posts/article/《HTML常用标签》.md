---
title: 《HTML常用标签》
date: 2020-02-13 09:48
tags:
  - 博客
  - 前端
  - html
categories:
  - 博客
  - 前端
---

# 一、a标签


```html
<a>内容</a>
<a href="" target="">内容</a>
```



## href属性


+ 网址  
[https://google.com](https://google.com)  
[http://google.com](http://google.com)  
[//google.com](//google.com)
+ 路径  
/a/b/c/index.html
+ 伪协议  
javascript:代码;  
mailto:邮箱  
tel:手机号
+ id  
href="#名字"



## target属性


+ _balnk  
新页面打开
+ _self  
默认页面打卡
+ _top  
在顶层页面打开
+ _parent  
在此页面的上一层页面打开
+ 自己命名  
windows的name  
iframe的name



# 二、img标签


```html
<img src="" alt="">
```



## src属性


相对路径  
绝对路径  
网址



## alt属性


加载失败显示的文字



## height属性


图片高度



## width属性


图片宽度



## 事件


+ onload事件  
加载成功时执行
+ onerror事件  
加载失败时执行



##响应式  
max-width: 100%



# 三、table标签


```html
<table><!-- 表格整体 -->
    <thead><!-- 表格头部 -->
        <tr><!-- 行 -->
            <th></th><!-- 单元格 -->
            <th></th>
            ...
        </tr>
    </thead>
    <tbody><!-- 表格主体 -->
        <tr>
            <td></td>
            <td></td>
            ...
        </tr>
        ...
    </tbody>
    <tfoot><!-- 表格底部 -->
        <tr>
            <td></td>
            <td></td>
            ...
        </tr>
    </tfoot>
</table>
```



## 相关样式


+ table-layout  
auto：根据单元格内容调整跨宽度  
fixed：根据单元格内容调整宽度，并且单元格内容平均
+ border-collapse: collapse  
合并单元格之间的间隙
+ border-spacing  
调整单元格之间的间隙



# 四、form标签


```html
<form action="" method="" autocomplete="" target="">
        内容
</form>
```



## action属性


谁来处理提交的数据  
如：xxx.php



## method属性


+ get  
表单数据会附加在 action 属性的URI中，并以 '?' 作为分隔符，然后这样得到的 URI 再发送给服务器
+ post  
表单数据会包含在表单体内然后发送给服务器



## autocomplete属性


+ on  
打开自动填充
+ off  
关闭自动填充



## target属性


用来指示在提交表单之后，在哪里显示收到的回复



+ _blank  
新页面
+ _self  
当前页面
+ windows或iframe的name  
程序猿自定义窗口名字接收

