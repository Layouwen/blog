---
title: npm 包推荐
date: 2024-03-22 23:11:34
tags:
  - 汇总
  - npm
categories:
  - 汇总
---

## Node默认包

| 包名                          | 描述                              |
| --------------------------- | ------------------------------- |
| util                        | 有 promisify 等工具                 |
| fs                          | 文件读写库，可以.promises 获取 promise 版本 |
| child_process               | 创建子进程                           |
| readline                    | 检测命令行回车事件                       |
| events                      | 订阅发布库                           |
| prettier                    | 代码格式化工具                         |
| prettier-plugin-tailwindcss | Tailwindcss 的 prettier 插件       |

## 功能类

| 包名               | 描述           |
| ---------------- | ------------ |
| @locator/runtime | 从浏览器快速定位到编辑器 |
|                  |              |

## 命令行工具

| 包名              | 描述                           |
| --------------- | ---------------------------- |
| concurrently    | windows 同时执行两个命令             |
| create-next-app | 构建next脚手架                    |
| yo              | 自定义脚手架平台，可自定义模版和下载指定模版       |
| anywhere        | 万金油服务器启动                     |
| ts-node         | 执行 ts 程序                     |
| node-dev        | 监听 node 程序                   |
| ts-node-dev     | 监听 ts 程序，更新自动重启              |
| nodemon         | js 文件变动重新运行                  |
| supervisor      | 监听 node 程序                   |
| tsc             | 编译 ts 为 js 文件                |
| cloc            | 统计代码行数                       |
| parcel          | 轻量打包工具，启动本地服务                |
| http-server     | 本地服务                         |
| server          | 本地服务                         |
| git-open        | 快速打开 git 仓库                  |
| json-server     | 基于 rest 规则生成服务器              |
| cross-env       | 跨平台设置环境变量                    |
| gh-pages        | 自动部署github page              |
| mitt            | Vue3推荐使用的eventbus            |
| eventemitter3   | 比较老牌的eventbus                |
| @novu/node      | node 端的消息(邮件/facebook...)通知库 |
| release-it      | 发版工具                         |

## 全端通用工具

| 包名              | 描述                               |
| --------------- | -------------------------------- |
| @faker-js/faker | 比较新的 mock 数据库                    |
| mathjs          | 计算库，包含大数值和小数计算问题解决方案             |
| crypto-js       | 加密库（md5、对称加密等）                   |
| qs              | 处理 url 路径 search 参数为对象           |
| flyio           | http 请求库                         |
| moment          | 格式化时间                            |
| dayjs           | 格式化时间                            |
| date-fns        | 比 dayjs 更新的时间工具                  |
| date-fns-tz     | 兼容性较强的, 指定时区格式化                  |
| async           | 异步库                              |
| civet           | 结合 typescript 和 coffescript 的新框架 |
| lodash-es       | 工具集 lodash 的 es 版本               |
| radash          | lodash 的替代品                      |
| zod             | 数据验证的工具                          |
| zod-i18n-map    | zod 的国际化版本                       |
| yup             | 数据验证工具                           |
| valibot         | 比 zod 更新的数据验证工具                  |
| fast-xml-parse  | 全端通用的 xml parse 库                |
| p-limit         | 并发控制库                            |
| core-js         | 各版本 js 实现, 垫片                    |
| globby          | 更好的 glob 匹配                      |
| web-worker      | 统一浏览器和nodejs的worker写法            |
|jspdf | pdf 生成库|
|docx| doc 文档生成|


## 浏览器工具

| 包名                           | 描述                                      |     |
| ---------------------------- | --------------------------------------- | --- |
| nanoid                       | 生成随机id                                  |     |
| @fingerprintjs/fingerprintjs | 获取浏览器唯一指纹                               |     |
| mixpanel-browser             | 埋点数据分析库客户端                              |     |
| @neodrag/vanilla             | js轻量拖拽库                                 |     |
| @tanstack/react-router       | 路由库 js/ts/react/vue/solid/svelte 等      |     |
| highlight.js                 | 高亮代码块                                   |     |
| marked                       | markdown 编辑器                            |     |
| subjx                        | 拖动、缩放、旋转                                |     |
| copy-to-clipboard            | 前端复制剪切板                                 |     |
| html2canvas                  | 通过 html 生成 canvas                       |     |
| canvas2Image                 | 把 canvas 转图片                            |     |
| fullpage.js                  | 全屏滚动库                                   |     |
| animejs                      | 轻小型动画库                                  |     |
| sentry                       | 前端异常监控系统                                |     |
| aegis                        | 前端异常监控系统，tdesign在使用                     |     |
| nprogress                    | 网页顶部进度条样式                               |     |
| html-to-text                 | 将标签内容转为text内容，去掉所有标签元素                  |     |
| gsap                         | 牛逼的动画效果库                                |     |
| @blocksuite/editor           | 好看的编辑器                                  |     |
| driver.js                    | 步骤提示库, 引导用户操作                           |     |
| @n1rjal/pm_ts                | 根据 postman 的 json 文件生成 typescript 的类型文件 |     |
| @builder.io/partytown        | 通过 WebWorker 加载第三方脚本                    |     |
| @emailjs/browser             | 无需后端即可完成邮件发送                            |     |
| pixi.js                      | canvas 与 webgl 结合的框架                    |     |
| @trpc/client                 | trpc 统一前后端的 typescript 类型               |     |
| quill                        | 富文本工具(国外)                               |     |
| @wangeditor/editor           | 富文本工具(国内)                               |     |
| particles.js                 | 粒子效果装饰库                                 |     |
| @editorjs/editorjs           | 类似 notion 的编辑器                          |     |
| pdfjs-dist                   | react较好的pdf预览组件                         |     |
| pdf-viewer                   | 浏览 pdf 的库                               |     |
| @trpc/client                 | trpc 客户端                                |     |
| underscore                   | 填充一些浏览器中的一些通用函数, 在旧版本中                  |     |
| @builder.io/partytown        | 将资源密集的操作转到 web worker 中                 |     |
| watermark-js-plus            | 暗水印添加                                   |     |
| cropperjs                    | 图片裁剪                                    |     |
| hotkeys-js                   | 快捷键库                                    |     |
| html2canvas                  | dom 转 canvas 库, 可用于截图                   |     |
| qrcode                       | 二维码生成                                   |     |
| node-qrcode                  | 支持多端的二维码生成                              |     |
| file-saver                   | 保存文件                                    |     |
| tailwind-merge               | 合并重复的 tailwind 类名                       |     |
| three                        | 基于 webgl 的 3d 库                         |     |
| stats.js                     | threejs 的性能检测面板                         |     |
| dat.gui                      | threejs 的 gui 修改参数插件                    |     |
| three-orbitcontrols          | threejs 相机移动控件                          |     |
| @tweenjs/tween.js            | 补间动画, 类似 css 的关键帧动画                     |     |
| babylonjs                    | 用于游戏开发的 webgl 3d 库                      |     |
| chart.js                     | 国外评分最高的图表库                              |     |
| effector                     | 比较新颖的状态库, 局部和全局. 支持 react / vue / solid |     |
| xstate                       | 基于状态机的 状态管理库                            |     |
| @apollo/client               | apollo 客户端, graphql                     |     |
| radar-sdk-js                 | 较为实惠的 google 地图替代方案                     |     |
| leader-line-new              | 动效引导线                                   |     |
| jheat.js                     | 类似 github 的热力图                          |     |
| JsBarcode                    | 条形码生成                                   |     |

## Node端工具

| 包名                   | 描述                         |
| -------------------- | -------------------------- |
| dotenv               | 读取 .env 文件                 |
| mstts-js             | 文字转语音                      |
| node-schedule        | 定时器工具                      |
| async-validator      | 校验库                        |
| md5                  | md5 加密                     |
| spark-md5            | 提供更多的 md5 方式               |
| glob                 | 通过通配符匹配路径                  |
| yargs-parser         | 获取 cmd 参数                  |
| chalk                | cmd 信息样式设置                 |
| cli-progress         | node 进度条                   |
| inquirer             | 命令行选项                      |
| commander            | 定制命令行界面                    |
| clear                | 清空控制台                      |
| figlet               | 打印字符文字                     |
| download-git-repo    | 下载 git 仓库                  |
| ora                  | 命令行加载动画                    |
| iconv-lite           | 解决编码问题                     |
| cheerio              | 可以理解为 node 下的 jQuery       |
| request              | 用于在 Node 端发送请求             |
| nodemailer           | 发送邮件                       |
| fs-extra             | 文件操作（移动文件等）                |
| uuid                 | uuid 库                     |
| ogr2ogr              | 文件格式转换，例如 dxf              |
| dxf                  | dxf 解析工具，可以转 svg           |
| gh-pages             | github page部署库             |
| decompress           | 解压压缩包                      |
| compress-images      | 图片压缩                       |
| imagemin             | 图片压缩                       |
| picocolors           | 输入颜色字符                     |
| log4js               | 生成日志文件                     |
| @trpc/server         | trpc 统一前后端的 typescript 类型  |
| sharp                | 图像处理库                      |
| morgan               | http 日志记录库                 |
| zlib                 | gzip 压缩解压缩库                |
| oxlint               | 替代 eslint 的库               |
| @antfu/eslint-config | antfu 推荐的 eslint config    |
| @trpc/server         | trpc 服务端                   |
| consul               | 微服务注册关联管理库                 |
| npmlog               | 在 cli 中的 log 工具            |
| semver               | 比较版本号大小                    |
| colors               | 颜色库                        |
| root-check           | 降级 sudo 权限, 避免权限造成的影响      |
| user-home            | 获取用户主目录路径                  |
| path-exists          | 判断文件 path 是否存在             |
| minimist             | 命令行参数解析                    |
| url-join             | url 的自动拼接                  |
| npminstall           | cnpm 中通过代码 install 的库      |
| rimraf               | rm 多系统删除库                  |
| simple-git           | 代码进行 git 管理                |
| bcrypt               | 加密库                        |
| figlet               | FIG 字符, 终端绘制字符画            |
| lru-cache            | lru 缓存算法的实现库               |
| workerpool           | worker 封装库                 |
| node-html-parser     | html 的解析库                  |
| docusaurus           | 文档自动生成工具                   |
| hpp                  | 参数脏数据清理                    |
| http-status          | http 状态的枚举定义库              |
| ioredis              | node-redis 的改良版            |
| @apollo/server       | graphql 的实现标准, 支持多个 web 框架 |
| fast-folder-size     | 快速获取文件夹的大小                 |
| passport             | 身份验证库                      |
| svg2pdf.js           | svg 转 pdf 文件               |
| pdf2html             | pdf 文件转 html               |
| ink                  | 用 react 编写 cli 界面          |


### 代码规范相关

| 包名                  | 描述                 |
| ------------------- | ------------------ |
| husky               | 哈士奇，用于 githook     |
| commitizen          | 用于 commit log 生成   |
| cz-customizable     | 自定义 cz 命令行的展示文本    |
| validate-commit-msg | 用于校验 commit log    |
| commitlint          | 用于校验 commit 的 lint |
|                     |                    |

### 服务端相关

| 包名                                | 描述                                                                                                      |
| --------------------------------- | ------------------------------------------------------------------------------------------------------- |
| egg-router-group                  | egg 的路由分组                                                                                               |
| egg-mongoose                      | egg 连接 MongoDB                                                                                          |
| egg-valiadate                     | egg 校验前端传递参数                                                                                            |
| co-wechat                         | 微信体系                                                                                                    |
| co-wechat-api                     | 微信体系 api                                                                                                |
| jsonwebtoken                      | token 库                                                                                                 |
| mysql                             | mysql 库                                                                                                 |
| mysql2                            | mysql 增强版库，里面有 promise 版本 mysql2/promise                                                                |
| sequelize                         | orm 操作数据                                                                                                |
| drizzle-orm                       | 新的 orm 数据库检索工具                                                                                          |
| mongodb                           | mongo 数据库操作                                                                                             |
| mongoose                          | 基于 mongodb 封装简化操作                                                                                       |
| svg-captcha                       | 验证码                                                                                                     |
| gm                                | 使用 [GraphicsMagick](http://www.graphicsmagick.org/) or [ImageMagick](http://www.imagemagick.org/) 生成缩略图 |
| body-parser                       | 解析body的内容                                                                                               |
| koa2-connect-history-api-fallback | koa中适配spa应用的history模式                                                                                   |
| express                           | 后端框架                                                                                                    |
| express-validator<br>             | express 请求入参校验                                                                                          |
| ipstack                           | 各种工具 api 集成平台                                                                                           |

#### Koa

| 包名               | 描述           |
| ---------------- | ------------ |
| koa-router       | koa 路由库      |
| @koa/bodyparser  | koa body 处理  |
| @koa/cors        | koa cors 库   |
| @types/koa__cors | koa cors 库类型 |

### babel相关

| 包名                       | 描述            |
|--------------------------|---------------|
| @babel/core              | babel 核心      |
| @babel/preset-env        | 解析 es         |
| @babel/preset-react      | 解析 react      |
| @babel/preset-typescript | 解析 typescript |

### Webpack相关

#### 工具

| 包名                 | 描述               |
| ------------------ | ---------------- |
| webpack-cli        | webpack 的 cli 工具 |
| webpack-dev-server | 热更新              |
|                    |                  |

#### loader

| 包名                        | 描述                      |
| ------------------------- | ----------------------- |
| stylus-loader             | stylus 加载器              |
| ts-loader                 | ts 加载器                  |
| awesome-typescript-loader | ts-loader 的强化版          |
| babel-loader              | babel 加载器               |
| vue-loader                | vue 加载器                 |
| loader-utils              | 在 loader 中获取 options 的库 |

#### plugin

| 包名                      | 描述         |
| ----------------------- | ---------- |
| mini-css-extract-plugin | 抽离 css 的文件 |
| html-webpack-plugin     | html 模板插件  |
| webpack-bundle-analyzer | 打包分析工具     |

## React 类

### 不再推荐

| 包名         | 描述                |
| ---------- | ----------------- |
| classnames | 类名写法优化(被 clsx 平替) |

### 工具

| 包名                                    | 描述                                      |
| ------------------------------------- | --------------------------------------- |
| react-hook-form                       | 评分较高的 form 表单封装库                        |
| clsx                                  | 类名写法优化                                  |
| braft-editor                          | 富文本编辑器                                  |
| rc-form                               | （类组件）react 表单处理，提供一些列 api（检验，默认值，数据读写）等 |
| react-spring                          | 交互动画库                                   |
| react-error-boundary                  | 错误边界库                                   |
| recoil                                | react 状态管理库                             |
| react-redux                           | react 封装的 redux 库                       |
| @emotion/styled @emotion/react        | css-in方案                                |
| react-helmet                          | 网站head相关的设置                             |
| @welldone-software/why-did-you-render | 查看是谁渲染                                  |
| immer                                 | 数据不可变，直接改值                              |
| redux-toolkit                         | 使用immer与react-redux结合，最佳实践              |
| swr                                   | 与上面类似的兄弟库                               |
| react-beautiful-dnd                   | 拖拽库                                     |
|@dnd-kit/core|拖拽库|
|sonner|比较普适的消息提示弹窗|
| react-dragable                        | 拖拽库                                     |
| gridstack                             | 栅格化拖拽库                                  |
| react-grid-layout                     | 栅格化拖拽库                                  |
| rc-resize-observer                    | 大小监听组件                                  |
| react-email                           | 邮件组件模板                                  |
| swr                                   | 接口请求库                                   |
| react-transition-group                | 动画库                                     |
| react-animations                      | 动画库                                     |
| react reveal                          | 动画库                                     |
| tweenone                              | 动画库                                     |
| immutabale.js                         | 数据不可变库, 反直觉                             |
| react-slick                           | 滑块轮播图组件, antd 内部使用的这个                   |
| react-icons                           | react 用的 icon 图标集                       |
| framer-motion                         | 丝滑的动画库                                  |
|motion|动画库|
|wouter|一个轻量的route库|
|avvvatars-react|精美的头像占位符|
|react-i18next|i18 库|
|recharts|基于 d3 的 chart 图表库|
| react-typewriter                      | react 模拟打字动画的库                          |
| react-joyride                         | react 引导下一步流程库                          |
| @react-pdf/renderer                   | react pdf 渲染库                           |
| swiper                                | react 的轮播库                              |
| @tanstack/react-query                 | react 异步状态管理库                           |
| @react-spring/web                     | react 的动画库                              |
| react-top-loading-bar                 | 顶部加载条                                   |
| react-image-cropper                   | 图片裁剪库                                   |
| react-hot-toast                       | 消息提示组件                                  |
| react.qrcode                          | 二维码生成                                   |
| tldraw                                | 画板库                                     |
| @blocknote/core                       | 类似 notion 的富文本库                         |
| react-error-boundary                  | 错误捕获                                    |
| react-confetti                        | 庆祝的烟花纸屑动画                               |
| react-lazy-load-image-component       | 图片懒加载库                                  |
| react-window-infinite-loader          | 加载更多库                                   |
| react-window                          | 虚拟滚动库                                   |
| lucide-react                          | lucide 图表库的 react 实现                    |
| formik                          | 小巧的表单处理库                    |


## ReactNative

| 包名                             | 描述        |
| ------------------------------ | --------- |
| react-navigation               | 导航条       |
| react-native-looped-carousel   | 轮播图       |
| react-native-refresh-list-view | 上拉加载和下拉刷新 |

## Next.js 类

| 包名           | 描述                       |
| ------------ | ------------------------ |
| next-intl    | 国际化库                     |
| server-only  | 防止服务端组件, 在客户端渲染          |
| next-auth    | 集成常用的授权应用                |
| next-sitemap | SEO 优化, 方便 google 检索你的网站 |
| next-seo | SEO 优化, 生成一些常见的 metadata |
| next-pwa | 快速创建离线应用 |


### Next.js 最佳实践框架

| 包名            | 描述                     |
| ------------- | ---------------------- |
| t3-app@latest | ts 一个推崇类型安全的 next 实践框架 |

## UI 库

| 包名            | 描述                      |
| ------------- | ----------------------- |
| @mui/material | mui 谷歌风格的 material 组件库            |
|@mui/joy|mui 自己设计的组件库|
| Mantine       | Mantine UI 库            |
| shadcn/ui     | 使用 tailwindcss 编写, 本地组件 |

## Vue 类

| 包名                | 描述     |
|-------------------|--------|
| vue               | vue 核心 |
| @vue/compiler-sfc | 单文件编译器 |

### 工具

| 包名                  | 描述                      |
|---------------------|-------------------------|
| vuedraggable        | vue的拖拽库                 |
| vue-clipboard3      | 剪切板                     |
| vuex-persist        | vuex持久化数据               |
| vue-reuse-template  | 组件内重用部分模板 - 安法师 (非组件)   |
| unplugin-vue-macros | 组件内重用部分模板               |
| @tanstack/vue-query | 对标 react-query 的异步状态管理库 |

## 环境类

| 包名         | 描述          |
|------------|-------------|
| typescript | ts 环境       |
| zx         | 用js编写bash脚本 |

## 构建优化

| 包名                | 描述                 |
|-------------------|--------------------|
| @million/compiler | 自动优化构建的代码, vite 支持 |

# Mock 服务

| 包名  | 描述            |
| --- | ------------- |
| msw | 比较新颖的 mock 服务 |

# 测试

## 基础库

| 包名             | 描述                  |
| -------------- | ------------------- |
| flush-promises | 将所有 promise 状态都改为完成 |
|                |                     |
## E2E
| 包名         | 描述                    |
| ---------- | --------------------- |
| nightwatch | 基于 webdriver.js 的测试框架 |
| cypress    | 高度内聚的集成测试框架           |
| testcafe   | 傻瓜智能的测试框架             |
## 后端

| 包名        | 描述         |
| --------- | ---------- |
| supertest | 测试 http 的库 |

# 小程序

| 包名                | 描述                |
| ----------------- | ----------------- |
| weapp-tailwindcss | 小程序支持 tailwindcss |

# 全栈框架

| 包名             | 描述                          |
| -------------- | --------------------------- |
| TanStack Start | tanstack 的全栈框架, 目前是 beta 版本 |
| Nextjs         | react技术栈                    |
| Nuxtjs         | vue技术栈                      |
| Astro          | 领先的分岛框架                     |

# 文档构建

|包名|描述|
| ----------------- | ----------------- |
| docusaurus | 国外比较流行的 md 文档网站构建 |