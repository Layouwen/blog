---
title: JetBrains 篇
date: 2024-01-23T15:23:28.000Z
tags:
  - 汇总
  - jetbrain
categories:
  - 汇总
  - 开发环境
uuid: e77a9706-bfc9-40be-9816-99efeb74227c
---

# 配置

## Color Scheme

Material Darker

### Color Scheme Font

Font —— jetBrains Mono
Size —— 16.0

### Console Font

Font —— jetBrains Mono

### Advanced Settings

选中 Disable double modifier key shortcuts 

# 插件

## 通用

Sweep AI - 

Junie - 

inlineError —— 内联显示 error 信息

GitToolbox —— 更好的查看Git记录

IdeaVim —— Vim模拟器

IdeaVim-EasyMotion —— 模拟vim-easymotion

IdeaVimExtension —— vim一些插件拓展

AceJump —— 类似VIM的位置跳转

Material Theme UI —— 主题

Atom Material Icons —— 更好看的文件图标

Key Promoter X —— 显示鼠标指向功能的快捷键

String Manipulation —— 字符串命名格式转换

Quick File Preview —— 快速文件预览

Randomness —— 随机生成器

GitHub Copilot —— Github代码自动提示

CodeGlance3 —— 代码走廊

WakaTime —— 记录代码时间

LeetCode Editor —— LeetCode插件

.ignore —— ignore插件

Murphysec Code Scan —— 第三方依赖安全检测

chatgpt —— chatgpt插件

Codeium - 免费的 ai 补全

Sourcegraph Cody - 免费的 ai 补全

TONGYI Lingma - 阿里巴巴的通义免费 ai 补全

Mintlify Doc Writer - 通过 ai 解释你的代码, 编写一些注释文档

MarsCode:Al Coding Assistant - 字节跳动豆包出品的 ai 助手

Continue - 上下文 ai 模型对话生成

## 部分有

AiXcoder Code Completer —— 代码补全和代码搜索工具

## WebStrom

Cypress Support —— e2e测试框架

Codota —— AI补全代码

## 自选

Fig —— fig 命令行补全依赖插件

Rainbow Brackets —— 彩虹括号

Indent Rainbow —— 彩虹缩进

Nyan Progress Bar —— 彩虹猫进度条

IDE resset —— 重置激活时间  

EditorConfig —— editor 统一项目成员的编辑器规范（新版自带）

Makefile Language —— 语法提示（新版自带）

Settings Repository —— 通过 repo 同步配置文件（2022.3 版本以上需要自行安装该插件）

### 个别需要补充的插件说明

#### Github Copilot

强制显示代码提示 `option + \`

向左切换建议 `option + [`

向右切换建议 `option + ]`

#### Translation

`ctrl + shift + Y` 翻译当前单词

#### AceJump

`ctrl + ;` 接着输入需要搜索的关键字

`backspace` 退格键清空搜索关键字，重新输入

`tab` 查看下一页的搜索结果

`ESC` 退出搜索状态

`enter` 跳转到下一个结果

`shift + enter` 跳转到上一个结果

> 如果你是ideaVim用户可以设置以下配置
> mac/linux 编辑 .ideavimrc
> windows 编辑 ._ideavimrc

```bash
" Press `f` to activate AceJump
map f :action AceAction<CR>
" Press `F` to activate Target Mode
map F :action AceTargetAction<CR>
" Press `g` to activate Line Mode
map g :action AceLineAction<CR>
```

#### String Randomness

`alt + r` 启动随机生成器

# 特殊问题

## rpx报错问题

### 方案1 使用sass函数

取消勾选 propetry value / element
Editor —— Inspetions —— CSS ——Invalid CSS propetry value 和 Invalid CSS element

或者使用scss，创建一个fn.scss
```scss
@function rpx($value) {
  @return $value * 1rpx;
}
```

### 方案2 使用file watchers

添加新规则

Name：css样式rpx空格处理
File type：Vue.js template 或者 Cascading Style Sheet
Scope：Project Files
Program：sed
Arguments：`-i "" s/"\ rpx"/rpx/g $FilePath$`
Output paths to refresh：`$FilePath$`

> 如果是 Windows  
> Arguments 换成 `-i s/"\ rpx"/rpx/g $FilePath$`  
> Program 需要填入 `sed` 的完整路径
> 下载地址：[https://github.com/mbuilov/sed-windows](https://github.com/mbuilov/sed-windows)


## 无法识别alias别名

配置 webpack 导入 alias 的 config

alias.config.js
```js
const resolve = dir => require('path').join(__dirname, dir)

module.exports = {
  resolve: {
    alias: {
      '@': resolve('src'),
    },
  },
}
```

## live template

**rfch**
```js
import React, { useEffect } from 'react';

interface $METHOD_NAME$Props {}

const $METHOD_NAME$: React.FC<$METHOD_NAME$Props> = (props) => {
  useEffect(() => {}, []);
  return <div>$END$</div>;
};

export default $METHOD_NAME$;

```
