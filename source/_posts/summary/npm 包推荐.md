---
title: npm 包推荐
date: 2024-03-22T23:11:34.000Z
tags:
  - 汇总
  - npm
categories:
  - 汇总
uuid: 8b82cdc2-57da-47ac-ac8d-c69597208347
---
## 1. Node.js 内置模块

| 包名              | 描述                                                       |
| --------------- | -------------------------------------------------------- |
| `fs`            | 文件系统模块，用于读写文件、目录操作；推荐优先使用 `fs/promises` 或 `fs.promises`。 |
| `util`          | Node 工具模块，常用能力包括 `promisify`、`inspect`、类型判断等。            |
| `child_process` | 创建和管理子进程，用于执行 shell 命令、启动外部程序等。                          |
| `readline`      | 逐行读取输入流，常用于命令行交互和处理大文件。                                  |
| `events`        | 事件发布订阅基础模块，提供 `EventEmitter`。                            |
| `zlib`          | 压缩和解压模块，支持 gzip、deflate、brotli 等格式。                      |

## 2. 语言、运行环境与脚本

| 包名 | 描述 |
| --- | --- |
| `typescript` | TypeScript 语言工具链，提供类型检查和编译能力。 |
| `ts-node` | 直接运行 TypeScript 文件，适合脚本和开发调试。 |
| `ts-node-dev` | 监听 TypeScript 文件变化并自动重启进程，可替代早期 Node 监听重启工具。 |
| `node-dev` | 监听 Node.js 程序文件变化并自动重启。 |
| `nodemon` | 通用进程监听重启工具，常用于 Node 服务开发，可替代早期的进程监听工具。 |
| `tsx` | 运行 TypeScript / ESM 脚本的轻量工具，可作为 `ts-node` 的现代替代方案。 |
| `zx` | 使用 JavaScript 编写 shell 脚本，适合自动化任务。 |
| `civet` | 类 CoffeeScript 的 TypeScript 方言，提供更简洁的语法。 |

## 3. 脚手架、命令行与开发辅助

| 包名 | 描述 |
| --- | --- |
| `create-next-app` | Next.js 官方脚手架，用于快速创建 Next 项目。 |
| `yo` | Yeoman 脚手架平台，可自定义 generator 生成项目模板。 |
| `concurrently` | 并行执行多个命令，适合同时启动前后端或多个开发服务。 |
| `cross-env` | 跨平台设置环境变量，解决 Windows/macOS/Linux 命令差异。 |
| `release-it` | 自动化发版工具，支持版本号、tag、changelog、npm 发布等流程。 |
| `cloc` | 统计代码行数和语言分布。 |
| `git-open` | 从命令行快速打开当前 Git 仓库的远程页面。 |
| `gh-pages` | 将构建产物发布到 GitHub Pages。 |
| `@locator/runtime` | 在浏览器中定位页面元素对应的源码位置，辅助前端调试。 |
| `react-scan` | React 渲染性能扫描和分析工具。 |

## 4. 本地服务与 Mock

| 包名 | 描述 |
| --- | --- |
| `http-server` | 快速启动静态文件服务器。 |
| `anywhere` | 简单易用的静态资源服务器。 |
| `serve` | 常用静态服务工具，适合预览构建产物。 |
| `json-server` | 基于 JSON 文件快速生成 REST API Mock 服务。 |
| `msw` | Mock Service Worker，可在浏览器或 Node 环境拦截请求并模拟接口。 |
| `parcel` | 零配置打包工具，也可用于快速启动本地开发服务。 |

## 5. 代码质量、格式化与工程规范

| 包名 | 描述 |
| --- | --- |
| `prettier` | 代码格式化工具，统一代码风格。 |
| `eslint` | JavaScript / TypeScript 代码检查工具。 |
| `oxlint` | Rust 编写的高性能 JavaScript / TypeScript linter。 |
| `@antfu/eslint-config` | Anthony Fu 维护的 ESLint 配置预设，适合现代前端项目。 |
| `husky` | 管理 Git hooks，常用于提交前检查、格式化、测试。 |
| `commitizen` | 交互式生成规范化 commit message。 |
| `cz-customizable` | 自定义 Commitizen 的交互选项和提交格式。 |
| `commitlint` | 校验 commit message 是否符合规范，可替代早期的 commit message 校验工具。 |

## 6. 通用工具库

| 包名 | 描述 |
| --- | --- |
| `lodash-es` | Lodash 的 ES Module 版本，适合现代打包工具按需 tree-shaking。 |
| `radash` | 现代 JavaScript 工具函数库，可作为 Lodash 的轻量替代方案。 |
| `async` | 异步流程控制工具库，适合队列、串并行任务编排。 |
| `p-limit` | 控制 Promise 并发数量。 |
| `globby` | 基于 glob 的文件路径匹配工具，API 更友好。 |
| `glob` | 通配符文件路径匹配库。 |
| `semver` | 解析和比较语义化版本号。 |
| `lru-cache` | LRU 缓存算法实现。 |
| `core-js` | JavaScript 标准库 polyfill，补齐旧环境缺失能力。 |
| `qs` | URL querystring 与对象之间的序列化/反序列化。 |
| `url-join` | 安全拼接 URL 路径片段。 |
| `nanoid` | 生成短小、URL 友好的唯一 ID。 |
| `uuid` | 生成 UUID。 |
| `mathjs` | 数学计算库，支持表达式、大数、矩阵等能力。 |
| `@faker-js/faker` | 生成模拟数据，例如姓名、地址、邮箱、日期等。 |

## 7. 日期、时间与国际化

| 包名 | 描述 |
| --- | --- |
| `dayjs` | 轻量日期处理库，API 类似 Moment，适合替代传统重量级日期库。 |
| `date-fns` | 函数式日期工具库，按需引入友好，适合替代传统重量级日期库。 |
| `date-fns-tz` | 基于 `date-fns` 的时区处理工具。 |
| `react-i18next` | React 国际化方案，基于 i18next。 |
| `next-intl` | Next.js 国际化库，适配 App Router / Server Components 场景。 |
| `lingo.dev` | 基于 LLM 的网站/应用翻译工具。 |
| `zod-i18n-map` | 为 Zod 校验错误提供国际化文案映射。 |

## 8. 数据校验、Schema 与类型安全

| 包名 | 描述 |
| --- | --- |
| `zod` | TypeScript 优先的 schema 校验库，可从 schema 推导类型。 |
| `yup` | 常用对象 schema 校验库，表单生态中较常见。 |
| `valibot` | 轻量、类型友好的 schema 校验库。 |
| `async-validator` | 异步数据校验库，常见于表单和 UI 组件库内部。 |
| `express-validator` | Express 请求参数校验中间件。 |
| `envalid` | 环境变量校验和类型转换工具。 |
| `hpp` | 清理 HTTP 参数污染，降低重复 query 参数带来的安全风险。 |

## 9. 网络请求、API 与全栈类型

| 包名 | 描述 |
| --- | --- |
| `flyio` | 支持多端的 HTTP 请求库。 |
| `@trpc/client` | tRPC 客户端，用于端到端类型安全的 API 调用。 |
| `@trpc/server` | tRPC 服务端，用于构建类型安全的 API 服务。 |
| `@apollo/client` | Apollo GraphQL 客户端。 |
| `@apollo/server` | Apollo GraphQL 服务端实现。 |
| `@n1rjal/pm_ts` | 根据 Postman JSON 生成 TypeScript 类型定义。 |
| `http-status` | HTTP 状态码枚举和说明。 |
| `ipstack` | IP 地理位置等 API 服务。 |

## 10. 安全、加密与身份认证

| 包名 | 描述 |
| --- | --- |
| `crypto-js` | 浏览器/Node 通用加密工具，支持 MD5、AES 等算法。 |
| `md5` | MD5 摘要工具。 |
| `spark-md5` | 支持增量计算的 MD5 工具，适合大文件分片场景。 |
| `bcrypt` | 密码哈希库，常用于用户密码存储。 |
| `jsonwebtoken` | 生成和验证 JWT。 |
| `passport` | Node.js 认证中间件，支持多种登录策略。 |
| `dompurify` | 清理 HTML 字符串，降低 XSS 风险。 |
| `svg-captcha` | 生成图形验证码。 |
| `root-check` | 避免 CLI 工具在 root 权限下运行，降低权限风险。 |

## 11. 文件、文档、图片与多媒体处理

| 包名 | 描述 |
| --- | --- |
| `fs-extra` | 增强版文件操作库，支持复制、移动、确保目录存在等常用能力。 |
| `decompress` | 解压 ZIP、tar 等压缩包。 |
| `rimraf` | 跨平台删除文件/目录，常用于清理构建产物。 |
| `path-exists` | 判断文件或目录是否存在。 |
| `fast-folder-size` | 快速计算文件夹大小。 |
| `iconv-lite` | 字符编码转换，常用于处理 GBK 等非 UTF-8 内容。 |
| `sharp` | 高性能图像处理库，支持缩放、裁剪、格式转换等。 |
| `imagemin` | 图片压缩工具链。 |
| `compress-images` | 图片压缩工具。 |
| `gm` | GraphicsMagick / ImageMagick 的 Node 封装，用于图像处理。 |
| `ogr2ogr` | 地理空间数据格式转换工具封装。 |
| `dxf` | DXF 文件解析工具，可用于 CAD 数据处理。 |
| `jspdf` | 在浏览器或 Node 中生成 PDF。 |
| `docx` | 生成 `.docx` Word 文档。 |
| `svg2pdf.js` | 将 SVG 转换为 PDF。 |
| `pdf2html` | 将 PDF 转换为 HTML。 |
| `pdfjs-dist` | Mozilla PDF.js 构建包，用于 PDF 渲染和预览。 |
| `html-to-text` | 将 HTML 转换为纯文本。 |
| `html2canvas` | 将 DOM 渲染为 canvas，常用于网页截图。 |
| `canvas2image` | 将 canvas 转换为图片。 |
| `file-saver` | 在浏览器中触发文件保存。 |
| `cropperjs` | 浏览器图片裁剪工具。 |

## 12. 命令行交互与日志

| 包名 | 描述 |
| --- | --- |
| `commander` | 构建命令行程序，支持命令、参数、帮助信息等。 |
| `inquirer` | 命令行交互式问答工具。 |
| `yargs-parser` | 解析命令行参数。 |
| `minimist` | 轻量命令行参数解析工具。 |
| `chalk` | 终端彩色文本输出。 |
| `picocolors` | 极轻量终端颜色库。 |
| `colors` | 终端颜色输出库。 |
| `ora` | 终端加载动画。 |
| `cli-progress` | 终端进度条。 |
| `clear` | 清空终端屏幕。 |
| `figlet` | 在终端生成字符画标题。 |
| `npmlog` | npm 风格日志工具。 |
| `log4js` | 日志记录库，支持文件输出、日志级别和格式化。 |
| `morgan` | HTTP 请求日志中间件，常用于 Express/Koa 服务。 |
| `ink` | 使用 React 构建命令行界面。 |

## 13. 后端框架与中间件

| 包名 | 描述 |
| --- | --- |
| `express` | Node.js Web 框架，生态成熟。 |
| `body-parser` | 解析 HTTP body 的 Express 中间件；新版本 Express 已内置类似能力。 |
| `koa-router` | Koa 路由库。 |
| `@koa/bodyparser` | Koa 请求体解析中间件。 |
| `@koa/cors` | Koa CORS 跨域处理中间件。 |
| `@types/koa__cors` | `@koa/cors` 的 TypeScript 类型声明。 |
| `koa2-connect-history-api-fallback` | 在 Koa 中支持 SPA history 路由回退。 |
| `egg-router-group` | Egg.js 路由分组工具。 |
| `egg-mongoose` | Egg.js MongoDB/Mongoose 集成。 |
| `egg-validate` | Egg.js 参数校验插件；原文为 `egg-valiadate`，建议核对包名。 |
| `co-wechat` | 微信公众平台相关能力封装。 |
| `co-wechat-api` | 微信 API 调用封装。 |
| `necord` | NestJS 与 Discord.js 集成库。 |
| `@upyo/core` | 面向 Node/Bun/Deno 的邮件发送抽象库。 |
| `nodemailer` | Node.js 邮件发送库。 |
| `@novu/node` | Novu 通知系统的 Node SDK，可用于邮件、短信、站内通知等。 |
| `discord.js` | Discord API SDK。 |
| `mstts-js` | Microsoft 文本转语音相关 SDK/封装。 |

## 14. 数据库、ORM 与服务发现

| 包名 | 描述 |
| --- | --- |
| `mysql` | MySQL Node 客户端。 |
| `mysql2` | MySQL 客户端增强版，支持 Promise API 和更好的性能。 |
| `sequelize` | Node.js ORM，支持 MySQL、PostgreSQL、SQLite 等数据库。 |
| `drizzle-orm` | 类型安全、轻量的现代 TypeScript ORM。 |
| `mongodb` | MongoDB 官方 Node.js 驱动。 |
| `mongoose` | 基于 MongoDB 的对象建模工具。 |
| `ioredis` | Redis 客户端，支持集群、哨兵等高级能力。 |
| `consul` | Consul 客户端，用于服务注册、发现和配置管理。 |

## 15. 构建工具、Babel 与 Webpack

### Babel

| 包名 | 描述 |
| --- | --- |
| `@babel/core` | Babel 核心编译器。 |
| `@babel/preset-env` | 根据目标环境转换现代 JavaScript 语法。 |
| `@babel/preset-react` | 编译 JSX 和 React 相关语法。 |
| `@babel/preset-typescript` | 编译 TypeScript 语法。 |

### Webpack

| 包名 | 描述 |
| --- | --- |
| `webpack-cli` | Webpack 命令行工具。 |
| `webpack-dev-server` | Webpack 本地开发服务器，支持热更新。 |
| `babel-loader` | 在 Webpack 中使用 Babel 编译代码。 |
| `ts-loader` | 在 Webpack 中编译 TypeScript，可替代早期 TypeScript loader。 |
| `vue-loader` | 在 Webpack 中处理 Vue 单文件组件。 |
| `stylus-loader` | 在 Webpack 中处理 Stylus 样式。 |
| `loader-utils` | Webpack loader 开发辅助工具。 |
| `mini-css-extract-plugin` | 将 CSS 抽离为独立文件。 |
| `html-webpack-plugin` | 生成 HTML 入口文件并自动注入资源。 |
| `webpack-bundle-analyzer` | 分析 Webpack 打包体积。 |

### 构建优化

| 包名 | 描述 |
| --- | --- |
| `@million/compiler` | 面向 React 的编译期优化工具，主要用于减少不必要渲染。 |
| `tailwind-merge` | 合并并消除冲突的 Tailwind CSS class。 |

## 16. 浏览器基础能力与交互

| 包名 | 描述 |
| --- | --- |
| `copy-to-clipboard` | 浏览器复制到剪贴板工具。 |
| `hotkeys-js` | 快捷键绑定库。 |
| `@fingerprintjs/fingerprintjs` | 浏览器指纹识别库。 |
| `mixpanel-browser` | Mixpanel 浏览器端埋点 SDK。 |
| `sentry` | 应用异常监控和性能追踪平台 SDK。 |
| `aegis` | 腾讯云前端监控 SDK。 |
| `nprogress` | 页面顶部加载进度条。 |
| `driver.js` | 用户引导和步骤提示库。 |
| `@builder.io/partytown` | 将第三方脚本转移到 Web Worker 中运行，降低主线程压力。 |
| `web-worker` | 统一浏览器和 Node.js 的 Worker 写法。 |
| `@emailjs/browser` | 浏览器端邮件发送服务 SDK。 |
| `radar-sdk-js` | 地图、地理围栏和位置服务 SDK，可作为 Google Maps 相关方案的替代选择。 |
| `watermark-js-plus` | 添加明水印或暗水印。 |

## 17. 动画、拖拽、图形与可视化

| 包名 | 描述 |
| --- | --- |
| `animejs` | 轻量 JavaScript 动画库。 |
| `gsap` | 专业级 Web 动画库，适合复杂时间轴和高质量动效。 |
| `@tweenjs/tween.js` | 补间动画库。 |
| `subjx` | DOM 元素拖拽、缩放、旋转工具。 |
| `@neodrag/vanilla` | 轻量无框架拖拽库。 |
| `leader-line-new` | 绘制元素之间的引导线或连线。 |
| `particles.js` | 粒子背景效果库。 |
| `pixi.js` | 2D WebGL/canvas 渲染引擎，适合游戏和复杂图形。 |
| `three` | WebGL 3D 渲染库。 |
| `stats.js` | 性能面板，常用于 Three.js FPS 监控。 |
| `dat.gui` | 可视化调参面板，常用于图形/Three.js demo。 |
| `three-orbitcontrols` | Three.js 轨道相机控制器；现代项目通常从 `three/examples` 或 `three/addons` 引入。 |
| `babylonjs` | WebGL 3D 游戏和渲染引擎。 |
| `chart.js` | 常用图表库，适合基础统计图表。 |
| `jheat.js` | 类 GitHub 贡献图的热力图组件。 |
| `recharts` | React 图表库，基于 D3 与 SVG。 |

## 18. 编辑器、Markdown、代码高亮与内容展示

| 包名 | 描述 |
| --- | --- |
| `highlight.js` | 代码高亮库。 |
| `marked` | Markdown 解析器。 |
| `quill` | 富文本编辑器。 |
| `@wangeditor/editor` | 国内常用富文本编辑器。 |
| `@editorjs/editorjs` | 块编辑器，适合类 Notion 的内容编辑体验。 |
| `@blocksuite/editor` | 块编辑器框架，适合构建文档/白板类产品。 |
| `@blocknote/core` | 类 Notion 的块编辑器核心库。 |
| `braft-editor` | React 富文本编辑器。 |
| `tldraw` | 白板/绘图编辑器。 |

## 19. 条码、二维码与标识

| 包名 | 描述 |
| --- | --- |
| `qrcode` | 生成二维码，支持浏览器和 Node，可覆盖多数二维码生成场景。 |
| `react.qrcode` | React 二维码组件，建议核对维护状态和包名。 |
| `JsBarcode` | 条形码生成库。 |
| `avvvatars-react` | 生成美观的 React 头像占位符。 |

## 20. React 生态

### 表单与数据请求

| 包名 | 描述 |
| --- | --- |
| `react-hook-form` | 高性能 React 表单库，适合复杂表单。 |
| `formik` | React 表单状态管理库，API 简洁。 |
| `rc-form` | 早期 React 表单库，主要用于类组件和旧项目。 |
| `@tanstack/react-query` | React 异步状态管理库，适合服务端数据缓存、请求状态和失效刷新。 |
| `swr` | React 数据请求和缓存库。 |

### 状态管理

| 包名 | 描述 |
| --- | --- |
| `react-redux` | React 官方 Redux 绑定。 |
| `redux-toolkit` | Redux 官方推荐工具包，内置 Immer，简化样板代码。 |
| `recoil` | React 状态管理库，支持 atom/selector 模型。 |
| `effector` | 响应式状态管理库，支持 React/Vue/Solid 等生态。 |
| `xstate` | 状态机和状态图管理库，适合复杂流程状态。 |
| `immer` | 使用可变写法生成不可变数据，适合替代重量级不可变数据结构库。 |

### 样式与 UI 辅助

| 包名 | 描述 |
| --- | --- |
| `clsx` | 条件组合 className 的轻量工具，适合替代传统 className 拼接库。 |
| `@emotion/react` | Emotion CSS-in-JS 核心包。 |
| `@emotion/styled` | Emotion 的 styled API。 |
| `lucide-react` | Lucide 图标库的 React 版本。 |
| `react-icons` | React 图标集合，覆盖多个图标库。 |

### 动画、拖拽与布局

| 包名 | 描述 |
| --- | --- |
| `framer-motion` | React 动画库，适合页面转场和复杂交互动效。 |
| `motion` | Motion 动画库，Framer Motion 的现代包形态。 |
| `react-spring` | 基于弹簧物理模型的 React 动画库。 |
| `@react-spring/web` | React Spring 的 Web 包。 |
| `react-transition-group` | React 进入/退出动画状态管理。 |
| `react-animations` | CSS 动画预设集合。 |
| `tweenone` | React 动画库，常见于 Ant Design 生态早期示例。 |
| `@dnd-kit/core` | 现代 React 拖拽库核心包，适合替代停止维护的 React 拖拽方案。 |
| `react-draggable` | React 拖拽组件；原文 `react-dragable` 建议修正。 |
| `gridstack` | 网格拖拽布局库。 |
| `react-grid-layout` | React 网格拖拽布局组件。 |
| `rc-resize-observer` | 监听元素尺寸变化的 React 组件。 |

### 组件、反馈与体验

| 包名 | 描述 |
| --- | --- |
| `sonner` | 现代 toast 消息提示组件。 |
| `react-hot-toast` | React toast 消息提示组件。 |
| `react-error-boundary` | React 错误边界组件。 |
| `@welldone-software/why-did-you-render` | 分析 React 组件重复渲染原因。 |
| `react-helmet` | 管理页面 `<head>` 内容，常用于标题和 meta。 |
| `react-slick` | React 轮播组件。 |
| `swiper` | 现代轮播/滑动组件库，支持多框架。 |
| `react-typewriter` | React 打字机效果组件。 |
| `react-joyride` | React 用户引导流程组件。 |
| `react-top-loading-bar` | React 顶部加载条组件。 |
| `react-confetti` | 礼花/纸屑动画组件。 |
| `react-lazy-load-image-component` | React 图片懒加载组件。 |
| `react-window` | React 虚拟滚动库。 |
| `react-window-infinite-loader` | 配合 `react-window` 实现无限加载。 |
| `react-image-cropper` | React 图片裁剪组件。 |
| `@react-pdf/renderer` | 使用 React 组件生成 PDF。 |
| `react-email` | 使用 React 编写邮件模板。 |
| `wouter` | 轻量 React 路由库。 |

## 21. Next.js 生态

| 包名 | 描述 |
| --- | --- |
| `next-auth` | Next.js 身份认证方案；新版本生态中也常称 Auth.js。 |
| `server-only` | 标记模块只能在服务端使用，避免被客户端组件误引用。 |
| `next-sitemap` | 为 Next.js 生成 sitemap 和 robots.txt。 |
| `next-seo` | 管理 SEO meta 信息，Pages Router 项目中更常见；App Router 可优先使用 Metadata API。 |
| `next-pwa` | 为 Next.js 添加 PWA 能力。 |
| `t3-app` | 类型安全优先的 Next.js 全栈项目模板/脚手架。 |

## 22. Vue / Nuxt 生态

| 包名 | 描述 |
| --- | --- |
| `vue` | Vue 核心框架。 |
| `@vue/compiler-sfc` | Vue 单文件组件编译器。 |
| `vuedraggable` | Vue 拖拽排序组件。 |
| `vue-clipboard3` | Vue 3 剪贴板工具。 |
| `vuex-persist` | Vuex 状态持久化。 |
| `vue-reuse-template` | 在 Vue 组件内复用模板片段。 |
| `unplugin-vue-macros` | Vue 宏能力集合，增强组件写法。 |
| `@tanstack/vue-query` | Vue 版本的 TanStack Query。 |

## 23. React Native 与小程序

| 包名 | 描述 |
| --- | --- |
| `react-navigation` | React Native 导航库。 |
| `react-native-looped-carousel` | React Native 轮播组件。 |
| `react-native-refresh-list-view` | React Native 上拉加载、下拉刷新列表组件。 |
| `weapp-tailwindcss` | 在微信小程序中使用 Tailwind CSS。 |

## 24. UI 组件库

| 包名 | 描述 |
| --- | --- |
| `@mui/material` | React Material Design 组件库。 |
| `@mui/joy` | MUI 团队推出的 Joy UI 组件库。 |
| `mantine` | React 组件库，覆盖表单、布局、hooks 等能力。 |
| `shadcn/ui` | 基于 Radix UI 和 Tailwind CSS 的可复制组件方案。 |
| `@douyinfe/semi-ui` | 抖音前端团队开源的 React 中后台组件库。 |
| `naive-ui` | Vue 3 组件库；原文 `naive` 建议核对为 `naive-ui`。 |
| `reka-ui` | Vue 无样式组件库，可用于构建类似 shadcn-vue 的组件系统。 |
| `shadcn-vue` | shadcn/ui 的 Vue 社区实现。 |
| `@nuxt/ui` | Nuxt 官方/生态 UI 组件库。 |

## 25. 全栈框架与文档站

| 包名 | 描述 |
| --- | --- |
| `Next.js` | React 全栈框架，支持 SSR、SSG、路由、Server Components 等能力。 |
| `Nuxt` | Vue 全栈框架，支持 SSR、SSG、文件路由等能力。 |
| `Astro` | 内容驱动的网站框架，主打 Islands Architecture。 |
| `TanStack Start` | TanStack 生态的全栈框架，仍处于快速演进阶段。 |
| `docusaurus` | 文档网站构建框架，适合技术文档、博客和产品文档站。 |

## 26. 测试工具

| 包名               | 描述                                    |
| ---------------- | ------------------------------------- |
| `flush-promises` | 等待当前队列中的 Promise 全部完成，常用于单元测试。        |
| `supertest`      | 测试 HTTP 服务接口，常用于 Express/Koa/Nest 服务。 |
| `cypress`        | 端到端和组件测试框架，集成度高。                      |
| `nightwatch`     | 基于 WebDriver 的端到端测试框架。                |
| `testcafe`       | 端到端测试框架，使用门槛较低。                       |
