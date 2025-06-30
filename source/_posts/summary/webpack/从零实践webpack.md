---
title: 从零实践webpack
date: 2023-06-30 17:18:54
tags:
  - 汇总
  - webpack
categories:
  - 汇总
---

```bash
mkdir zero2one-webpack
cd zero2one-webpack
pnpm init
pnpm i -D webpack webpack-cli @types/node style-loader css-loader csv-loader xml-loader toml yamljs json5 html-webpack-plugin webpack-dev-server express webpack-dev-middleware webpack-hot-middleware webpack-visualizer-plugin bundle-stats-webpack-plugin
```

```bash
pnpm i lodash
```

```
npx webpack
```

# 主要概念

entry —— 入口文件，从这个位置开始分析引用
output —— 输出文件
module —— 模块中可以定义多组规则，规则里面使用对应的加载器
loader —— 加载器
plugin —— 插件，拓展功能
mode —— 开发环境不压缩代码，生产环境会压缩代码

# Loader

loader 的加载顺序为从后往前。

## 加载 css

通过 `css-loader` 获取 css 文件的内容，在通过 `style-loader` 将 css 放到 header 中显示出来。

## 加载图片/字体

webpack5 自带了 `asset/resource` loader 用来加载

## 加载数据

使用 `csv-loader` 和 `xml-loader` 分别加载 csv/tsv 和 xml 的文件。 

> JSON 在 webpack5 内置了 loader 进行加载

### 不使用加载器，使用解析器进行加载数据

在 module 中，指定 type 为 json，接着使用对应的 parse 解析即可。

> toml yamljs json5

# Plugin 

plugin 为一个数组，接收不同插件的实例。

## html-webpack-plugin

自动创建 html 文件，如果更新出入口文件，自动生成对应的 script 标签引入。

# Source Map

`devtool: 'inline-source-map'`

开启 source map 可以生成一个映射表，方便在调试的时候对应实际的代码。在 mode 为开发模式的时候，代码虽然已经不压缩了，但是没有办法对应上实际的代码。

# Watch 文件变化

一种可以直接在 webpack 后面添加 --watch 进行监听文件变化自动重新编译。

上面那种方式不会自动刷新浏览器，通过 `webpack-dev-server` 可以使用 websocket 通知浏览器自动刷新。

配置 `wepback.config.js` 中的 devServer 和 optimzation 字段即可

## 使用 webpack-dev-middleware 配合 express 使用

`webpack.config.js` 中配置 `output.publicPublic: '/'`

在 express 中使用 `webpack-dev-middleware` 中间件，并指定相同的 `publicPath` 即可

> 目前只会重新编译不会刷新浏览器，需要搭配 webpack-hot-middleware
> WebStorm 需关闭 safe save，新版好像没办法关闭这个

# 分割代码

## 直接 entry 分割

这种方式会导致同一个内容被复制多份

## 分块并阻止复制

### 通过 dependOn 进行依赖选择

与 entry 分割基本一致，只是在 dependOn 中指定共享的 入口名

### SplitChunksPluign 自动分块

自动将文件中用到的通过 npm 包抽离成一个 chunk

## 动态导入

使用 `import()` 动态导入，自动分块

> 因为返回的是 `Promise` ，所以浏览器版本过低是需要使用垫片（promise-polyfill）

# 预请求/预加载

## preFetch 预请求

`import(/* webpackPrefetch: true */, './path/to/LoginModal.js')`

在浏览器空闲的时候去请求，当需要的时候可以立刻请求。

> 上面这句话会在浏览器 header 中添加
>  <link rel="prefet" href = "login-mode-chunk.js">

## preLoad 预加载

`import(/* webpackPreLoad: true */ 'echatLibray')`

预加载块会与父快并行加载，等父块加载完。马上进行加载。

# 缓存

## 设置 hash 值

通过在 output 时指定 [contenthash] 来设置他的哈希值。

## 切割包 hash

在 `splitChunks` 中定义 `cacheGroups` 字的的匹配规则和生成的包名

> 其中一个依赖 bundle 发生改变，全部的 hash 值都发生了改变。
> 需要配置 `optimization.moduleIdls: deterministic` 这样只会改变修改了的文件 

# CLI 参数

```bash
npx webpack --config webpack.config.js
```

# bundle 分析

## [webpack-visualizer](https://chrisbateman.github.io/webpack-visualizer/)

## [Webpack Chart](https://alexkuz.github.io/webpack-chart/)

## [推荐 webpack-bundle-anzlyer](https://github.com/webpack-contrib/webpack-bundle-analyzer)

## [推荐 bundle-stats](https://github.com/relative-ci/bundle-stats/tree/master/packages/webpack-plugin)

有个显示的 bug，需要将 tab 栏的 z-index 设置为 0

# webpack 构建库

指定好 `output.library` 打包的类型和打包后的调用变量名
通过 `externals` 忽略不想纳入打包中的外部依赖，同时该依赖为 dev 依赖

# 设置环境变量

```bash
npx webpack --env name=avan --env production
# {
#   name: 'avan',
#   production: true
# }
```

> 默认值为 true

# 优化构建性能

1. 按需使用开发模式和生产模式
2. 及时更新包管理工具的版本，提高解析速度
3. 使用最新版的webpack
4. 精确控制 loader 的匹配文件，test 关键字
5. 缩小 loader 的检索范围，include 关键字
6. 每个 lodaer 都有对应的启动时间，尽可能少的使用 loader
7. 如果不需要 npm link 的功能，关闭 `resolve.symlinks: false`
8. 如果使用的 plugin 不依赖与上下文（context）则关闭 `resolve.cacheWithContext: false`
9. 尽可能减少 `resolve.modules`、`resolve.extensions`、`resolve.mainFiles`、`resolve.descriptionFiles` 的项目数量，他们会增加文件系统的调用数量
10. 使用 `DllPlugin` 将改动小的代码单独编译，但会提高构建复杂性
11. 减小编译的总大小，如：更小的库、多页面使用 `SplitChunksPlugin` 、只编译开发的部分
12. 使用 `thread-loader` 将较大开销的内容放到线程池
13. 使用 webpack 的 `cache` 字段，并在依赖安装前删除缓存目录（`postinstall`）
14. 可以关闭不必要的插件，如：进度插件 `ProgressPlugin`
15. 监听文件不要使用其他工具，使用 `watch` 模式。有些花费比较大的监听，可以设置 `watchOptions.poll` 增加轮询间隔。
16. 按需设置 `devtool` 的配置，大多情况下 `eval-cheap-module-source-map` 是最好选择
17. 开发模式不建议使用的插件 `TerserPlugin`、`AggressiveSplittingPlugin`、`AggressiveMergingPlugin`、`ModuleConcatenationPlugin`
18. 开发模式不建议生成 `[fullhash]`/`[chunkhash]`/`[contenthash]`等关键字
19. 开启 `optimization.runtimeChunk: truo` 
20. 项目庞大的时候，不要关闭 `optimization` 中的 `removeAvailableModules`、`removeEmptyChunks`、`splitChunks` 这些开销很大
21. 必要时关闭输出信息 `output.pathinfo: false`
22. 不要使用 `8.9.10` ~ `9.11.1` 版本的 nodejs，Map 和 Set 存在性能回归
23. `ts-loader` 关闭类型检测 `options.transplieOnly: true`。使用 `fork-ts-checker-notifier-webpack-plugin` 和 `fork-ts-checker-webpack-plugin` 插件做检测。他会另开一个线程执行。
24. `babel` 尽可能减少预设或插件数量
25. `node-sass` 会阻塞线程，使用 `thread-loader` 设置 `workerParalleJobs: 2`

# 不同框架的热刷新功能

vue —— `vue-loader` `vue-template-compiler`
react —— `react-hot-loader`

# Tree Shaking 摇树

它虽然可以删除未使用到的 import 代码。但是因为有时候我们有些引入的代码是不需要而外引用的，比如：css样式，文件内部已经执行了代码等。

所以我们需要标记副作用，告诉哪些文件是特殊的。

kj如果确保全局都没有副作用，可以直接在 `package.json` 中配置 `"sideEffectsk": false` 表示所有文件都没有副作用，可以安心删除
或者通过正则匹配指定文件 `"sideEffects": ["./src/some-side-effectful-file.js", "*.css"]` 存在副作用的文件

## 发挥摇树的优势

1. 使用 `ES2015` 模块导入导出
2. 确保没有把代码编译为 `CommonJS` 模块（@babel/preset-env）
3. 在 `package.json` 配置 `sideEffects`
4. 开启 `production` 模式，以及相关判断减少包的大小

# 懒加载

`() => import(path)` 返回一个promise，如果需要使用，可以在 `.then` 中获取 `default` 的内容

# 使用 ts

使用 `ts-loader` 或者 `@babel/preset-typescript` 进行解析 ts 文件

ts 中遇到 webpack 内置变量报错可以

```ts
/// <reference types="webpack/module" />
console.log(import.meta.webpack);
```

# 配置 images 资源输出位置

`assetModuleFilename: 'images/[hash][ext][query]'`

> 需要在 loader 的时候匹配到图片，并使用 `asset/resource` 进行加载

# 指定特定文件的输出位置

```js
output: { // 输出文件
  ...
  assetModuleFilename: 'images/[hash][ext][query]' // 指定 image 文件的输出位置
},
```

# 使用内联导入方式，获取相应 url

```json
{
  test: /\.svg/,
  type: 'asset/inline'
}
```

# css文件优化

生产模式下可以使用 `MiniCssExtractPlugin.loader` 替换 `style-loader`。通过环境变量控制

# 配置别名

```json
{
  alias: {
    '@': path.resolve('./src')
  }
}
```