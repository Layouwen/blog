---
title: Homebrew安装及配置
date: 2020-07-11 13:42:09
tags:
  - 博客
  - homebrew
  - macos
categories:
  - 博客
  - macos
---


## 安装

在终端输入

```bash
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

选择下载源，这里推荐选择 中科大，输入 1

接着更新brew

```bash
brew update
```

## 常用命令

`brew cask install 软件名1 软件名2 软件名3` 安装软件
`brew cask search [关键词]` 列出所有可以安装的软件
`brew cask uninstall 软件名` 卸载软件
`brew install 环境1 环境2 环境3` 安装环境
`brew search [关键词]` 列出所有可以安装的环境
`brew uninstall 环境名` 卸载环境
`brew cask info 软件名` 查看相关软件的信息
`brew info 环境名` 查看相关环境的信息
`brew cask cleanup` 删除Homebrew下载的包
`brew cask list` 列出Homebrew安装的包
`brew cask update` 更新Homebrew Cask

## 使用示例

安装Git

```bash
brew install git
```

安装curl

```bash
brew install curl
```

安装openssl

```bash
brew install openssl
```

安装node

```bash
brew install node
```

安装yarn

```bash
brew install yarn
```

安装chrome软件

```bash
brew cask install chrome
```