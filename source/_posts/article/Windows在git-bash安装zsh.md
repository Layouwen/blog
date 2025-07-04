---
title: Windows在git-bash安装zsh
date: 2022-07-22 10:00
tags:
  - 博客
  - 工具
  - zsh
  - windows
categories:
  - 博客
  - 工具
---

# 前言

博主现在已经转 mac 用户。但是因为家里的台式机是是 win 装黑果遇到各种问题，最终还是放弃在家使用 mac 环境。
但是又用习惯了 zsh 的各种插件。所以开始琢磨如何在 win 中使用一套舒适的环境。
最开始我是使用 wsl 中的 linux 环境安装 zsh。但毕竟属于子系统，很多环境是不共享。因为我是一名前端工程师，当做桌面程序开发的时候。在 wsl 需要另外配一套环境启动。
后面在 google 查阅之后。发现 git-bash 中安装 zsh 即可在大多环境与 window 共享的前提下使用 zsh 的生态。

# 安装 git-bash

[https://git-scm.com/downloads](https://git-scm.com/downloads)

安装这个没什么好说的，按照提示点击下一步操作。如果不知道怎么配置，全部默认即可。

打开后是类似这个样子

![https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c1b9f2f96cca4912a6b7b31cc6fbf2a1~tplv-k3u1fbpfcp-zoom-1.image](https://pic1.zhimg.com/80/v2-be6c033a8d8f0c47a1a4b81f736034d1_720w.png)

# 下载 zsh 的包

[https://packages.msys2.org/package/zsh?repo=msys&variant=x86_64](https://packages.msys2.org/package/zsh?repo=msys&variant=x86_64)

下载 `zsh-5.8-5-x86_64.pkg.tar.zst` 文件。5.8-5 是版本号，当你看到这配文章的时候版本号可能已经发生改变，所以你只需要下载 `zsh-xxx-x86_64.pkg.tar.zst` 即可。

![https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d325eac0599b4eaea2fa374b345ae8a3~tplv-k3u1fbpfcp-zoom-1.image](https://pic3.zhimg.com/80/v2-16838b5c27b5025dec43c65a23381cd3_720w.png)

# 解压 zsh 压缩文件

这里推荐使用 [https://peazip.github.io/](https://peazip.github.io/) 进行解压。当然如果你有其他的解压工具能解压也行。

解压后你的文件中应该包含 `etc` 和 `usr` 类似字眼。将解压出来的所有文件，包含刚刚说的文件。复制到 git-bash 安装的根目录。可能会提示冲突，选择覆盖文件即可。

![https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f185d07fe38c41dfa0a11db13d30311e~tplv-k3u1fbpfcp-zoom-1.image](https://picx.zhimg.com/80/v2-dbb97af1fb3f900f1e44d2c7d2aa9186_720w.png)

![https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6314799d90714dbb8c558bad89fdfb8d~tplv-k3u1fbpfcp-zoom-1.image](https://pica.zhimg.com/80/v2-9929b1394ac9a6a1e3736c1a50ce3041_720w.png)

# 安装 oh-my-zsh

如果你跟我一样，决定默认使用 zsh。而不进入 bash。可以在 `.bashrc` 加上下面代码。

```bash
if [ -t 1 ]; then
  exec zsh
fi
```

![https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b3b82682481e463e83603572e4066ea7~tplv-k3u1fbpfcp-zoom-1.image](https://pic1.zhimg.com/80/v2-fcdc278894f89abbd4d15184c6754517_720w.png)

![https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1681b6efd62a4cebb7425ab84f263a75~tplv-k3u1fbpfcp-zoom-1.image](https://pic1.zhimg.com/80/v2-6f88f431319471875930394532079178_720w.png)

在终端输入下面指令。进入 zsh

```bash
zsh
```

安装 oh-my-zsh

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## 使用一些默认插件

编辑 `~/.zshrc` ，如果没有则会自动创建。

```bash
vim ~/.zshrc
```

> 上面的 vim 如果不会操作，你可以使用 code 在你的用户根目录打开 .zshrc 文件编辑是一样的效果。但是你都用 zsh 了我相信你都是会用 vim 的。

```bash
plugins=(
  git
  bundler
  dotenv
  macos
  rake
  rbenv
  ruby
)
```

上面的是写官方的插件，如果你需要使用其他插件只需要安装好后，回车换行添加尚对应插件名即可。

## 配置主题

同样是在 `~/.zshrc` 文件中配置，添加下面代码

```bash
ZSH_THEME="robbyrussell"
```

> 如果你想要使用其他主题，可以在这里查看对应的名字替换即可 [https://github.com/ohmyzsh/ohmyzsh/wiki/Themes](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)

如果你想每次使用都有新的体验，可以设置为随机主题

```bash
ZSH_THEME="random"
```

或者你想在指定的几个主题中随机，那么可以加上下面代码

```bash
ZSH_THEME_RANDOM_CANDIDATES=(
  "robbyrussell"
  "agnoster"
)
```

如果不想看到某个特别讨厌的主题，可以忽略它

```bash
ZSH_THEME_RANDOM_IGNORED=(
pygmalion
tjkirch_mod
)
```

> 注意！！！  
> 上面所有关于 `.zshrc` 的操作，修改都不会立即生效。你可以退出重新进入终端即可生效，或者执行 `source ~/.zshrc` 让他立刻生效。

配置完后的大概效果是这样

![https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6d5e0cf3f4f046e59eb751f5f6b6d3e9~tplv-k3u1fbpfcp-zoom-1.image](https://picx.zhimg.com/80/v2-795af57fac6246fe0d583e014d4e7afe_720w.png)

# 安装一些实用的插件

## 语法高亮

`zsh-syntax-highlighting` 他可以高亮你的代码提示，让你更直观的知道你的命令是否有输入错误

安装

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
```

## 自动补全/提示

`zsh-users/zsh-autosuggestions` 他可以在你历史指令中找到与你当前输入指令匹配的记录，并高亮显示，如果想直接使用，可以直接通过 右方向键 补全。

安装

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

在 `plugins` 中添加

```bash
plugins=( 
    # other plugins...
    zsh-autosuggestions
)
```

## 还有一些实用默认插件

- z —— 快速跳转路径
- sudo —— 按两次 ESC 快速添加 sudo 前缀

# 不太相关的内容

如果你想让终端更好看点，可以在 Store 中安装 Windows Terminal 美化你的终端。大概效果就是我的封面图。

![https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3b1bb7be94e544ec83ba80f076e9738e~tplv-k3u1fbpfcp-zoom-1.image](https://picx.zhimg.com/80/v2-0b832736190f75adf90e1a909a586272_720w.png)