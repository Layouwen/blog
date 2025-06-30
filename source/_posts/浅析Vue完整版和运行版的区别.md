---
title: 浅析Vue完整版和运行版的区别
date: 2020-03-18 20:51
tags:
  - 博客
  - 前端
  - vue
categories:
  - 博客
  - 前端
---

- 完整版和运行版的名字区别
- tenplate和render的使用方法
- 使用codesandbox快速生成Vue项目
- 总结



## 一、完整版和运行版的名字区别

可以去BootCDN里面引用，进去后直接搜索 `Vue` 即可。
[BootCnd官网](https://www.bootcdn.cn/)
完整版的后缀是 —— vue.js
运行版的后缀多了个runtime —— vue.runtime.js
所有版本都有压缩版，压缩版功能一致，只是体积相对要小 —— 版本.min.js

## 二、template和render的使用方法

- template

  完整版可以直接在HTML里面或者在template选项中直接写代码。它会自动解析

  例如：

  1. 在index.jhtml中添加一个id为app的标签

     ```html
     <div id="app"></div>
     ```

  2. 接着添加script标签，引入完整版的js文件

     ```javascript
     <script src="https://cdn.bootcss.com/vue/2.6.11/vue.min.js"></script>
     ```

  3. 然后在main.js里，直接把html代码写入template选项中

     ```javascript
     new Vue({
         el: '#app',
         template: `
     		<div>{{n}}</div>
     	`,
         data: {
             n: 0
         }
     })
     ```

  4. 运行Vue后，它会直接把n为0写入到app标签中

- render

  运行版，需要将HTML标签写入render中，让render来写入html中

  例如：

  1. 在index.jhtml中添加一个id为app的标签

     ```html
     <div id="app"></div>
     ```

  2. 接着添加script标签，引入运行版的js文件

     ```html
     <script scr="https://cdn.bootcss.com/vue/2.6.11/vue.runtime.min.js"><script>
     ```

  3. 然后在main.js里，用render函数来创建标签

     ```javascript
     new Vue({
         el: '#app',
         render(h){
             return h('div', this.n)
         },
         data: {
             n: 0
         }
     })
     ```

  4. 运行Vue后，达到完整版一样的效果

## 三、使用codesandbox快速生成Vue项目

新手想学习Vue的时候，可以借助codesandbox来进行快速的搭建Vue项目。省去自己安装和配置的时间。当然开发的时候，还是自己在电脑中一步一步配置好。

[Codesandbox官网](https://codesandbox.io/)

### - 搭建过程

1. 点击上面链接，进入官网

2. 建议不用注册，注册后好像是限制项目数量。点击Create Sandbox

   ![2.png](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/3/18/170edb94562ff101~tplv-t2oaga2asx-image.image)

3. 接着点击Vue即可创建

   ![3.png](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/3/18/170edb9456448812~tplv-t2oaga2asx-image.image)

4. 创建完后就可以开始编辑代码，在旁边还有即时预览的窗口

5. 写完导出，点击File——Export to ZIP

   ![5.png](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/3/18/170edb94599251f4~tplv-t2oaga2asx-image.image)

6. 导出后，解压下载好的压缩包，就是个本地文件了

## 四、总结

|               | Vue完整版                      | Vue非完整版                   | 评价                              |
| :------------ | :----------------------------- | :---------------------------- | :-------------------------------- |
| 特点          | 有compiler                     | 没有compiler                  | compiler占40%体积                 |
| 视图          | 写在HTML里或者写在template选项 | 写在render函数里用h来创建标签 | h是尤雨溪写好传给render的         |
| cdn引入       | vue.js                         | vue.runtime.js                | 文件名不同，生成环境后缀为.min.js |
| webpack 引入  | 需要配置alias                  | 默认使用此版                  | 尤雨溪配置                        |
| @vue/cli 引入 | 需要额外配置                   | 默认使用此版                  | 尤雨溪、蒋豪群配置                |

最佳使用的方法：

​	使用非完整版，配合vue-loader和vue文件

思路：

1. 保证用户体验，用户下载的JS文件体积更小，但只支持h函数
2. 保证开发体验，开发者可以直接在vue文件里写HTML标签，而不写h函数
3. 麻烦的代码让loader去写，vue-loader把vue文件里面的HTML转换为h函数

