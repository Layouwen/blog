---
title: Mac 系统装机配置指北
date: 2024-12-11T15:22:00.000Z
tags:
  - 汇总
  - mac
categories:
  - 汇总
  - Mac系统
uuid: 4e955826-a7f6-491a-b414-bd65a6e0d935
---

# 前置条件

允许任何来源

```bash
sudo spctl --master-disable
```

[下载梯子](https://www.efcloud.cc)

终端配置

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

复制 .zshrc 文件

[https://github.com/Layouwen/config-backup/blob/master/mac/.zshrc](https://github.com/Layouwen/config-backup/blob/master/mac/.zshrc)

> 用户名和一些文件路径需要替换一下

spaceship 主题需要执行下面的命令

```bash
git clone https://github.com/spaceship-prompt/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt" --depth=1

ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$ZSH_CUSTOM/themes/spaceship.zsh-theme"
```

安装 bun

```
curl -fsSL https://bun.sh/install | bash
```

安装 Code Rabbit CLI 

```bash
curl -fsSL https://cli.coderabbit.ai/install.sh | sh
```

下载 brew

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

如果需要使用清华国内源，先设置环境变量，在 `brew update`

设置按键时长
- keyboard
- Key repeat rate 和 Delay until repeat 全部向右拉到 fast

设置三脂拖拽
- Accessibility
- Pointer Control
- Use trackpad for dragging

开启轻点触发
- Trackpad
- 勾选 Tap to click

设置 F1-F12 为默认键
- keyboard
- Keyboard Shortcuts.
- Function Keys
- 打钩 'Use F1,F2...'

设置显示秒
- Control Center
- Display the time with seconds

关闭默认快捷启动栏使用 alfred 替换
- keyboard
- Keyboard Shortcuts.
- 取消 Show Spotlight search 和 Show Finder search window
- 配置 alfred 快捷键

## 无法按住持续连按

### 关闭 ApplePressAndHoldEnabled

```bash
# obsidian
defaults write md.obsidian ApplePressAndHoldEnabled -bool false
```

```bash
# For cursor
defaults write com.todesktop.230313mzl4w4u92 ApplePressAndHoldEnabled -bool false
```

```bash
# webstorm
defaults write com.jetbrains.WebStorm ApplePressAndHoldEnabled -bool false
```

```bash
# idea
defaults write com.jetbrains.intellij ApplePressAndHoldEnabled -bool false
```

```bash
# Alfred
defaults write com.runningwithcrayons.Alfred ApplePressAndHoldEnabled -bool false
```

```bash
# For VS Code
defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false
```

```bash
# For VS Code Insider
defaults write com.microsoft.VSCodeInsiders ApplePressAndHoldEnabled -bool false
```

```bash
# For VS Codium
defaults write com.visualstudio.code.oss ApplePressAndHoldEnabled -bool false
```

```bash
# For VS Codium Exploration users
defaults write com.microsoft.VSCodeExploration ApplePressAndHoldEnabled -bool false
```

```bash
# 全局设置
defaults write -g ApplePressAndHoldEnabled -bool false
```

```bash
# If necessary, reset global default
defaults delete -g ApplePressAndHoldEnabled
```

# brew相关

```bash
brew install webp
brew install pandoc
brew install cowsay
brew install zplug
brew install trash
brew install scc
brew install duf
brew install diff-so-fancy
brew install lolcat
brew install fzf
brew install figlet
brew install tmux
brew install graphicsmagick
brew install cmatrix
brew install ctop
brew install fastfetch

# ripgrep \
# bat \
# tldr \
# thefuck \
# nvm \
```

- scc 更好的统计代码量
- duf 更好的 diff
- diff-so-fancy diff 后的 grep 过滤管道
- fzf 全局查看文件
- figlet 输出字符英文画
- lolcat 彩色输出管道
- cmatrix 没什么用的终端屏保
- graphicsmagick 图片处理工具转换压缩等
- 10.15.x 支持不太友好
	- tldr 更好的查看 -h
	- bat 更好的 cat 替代
	- ripgrep 更好的 grep

### 注意事项

nvm 安装完后，需要创建 `~/.nvm` 以及更新 `~/.zshrc` 的配置

```bash
brew install --cask mqttx
brew install --cask firefox
brew install --cask postman
brew install --cask nutstore
brew install --cask soapui
brew install --cask jetbrains-toolbox
brew install --cask iina
brew install --cask adrive
brew install --cask baidunetdisk
brew install --cask wechatwebdevtools
brew install --cask visual-studio-code
brew install --cask ticktick
brew install --cask snipaste
brew install --cask sourcetree
brew install --cask sunloginclient
brew install --cask feishu
brew install --cask discord
brew install --cask tencent-lemon
brew install --cask neteasemusic
brew install --cask picgo
brew install --cask macdown
brew install --cask drawio
brew install --cask microsoft-edge
brew install --cask mongodb-compass
brew install --cask chatbox
brew install --cask steam
brew install --cask tencent-meeting
brew install --cask josm
brew install --cask hbuilderx
brew install --cask sublime-text
brew install --cask thunder
brew install --cask notion
brew install --cask 1password
brew install --cask bob
brew install --cask apifox
brew install --cask telegram
brew install --cask wechat
brew install --cask qq
brew install --cask wechatwork
brew install --cask google-chrome
brew install --cask font-jetbrains-mono
brew install --cask iterm2
brew install --cask obsidian
brew install --cask chatgpt

# 新版 mac 系统[]
brew install --cask docker
brew install --cask proxyman
brew install --cask dingtalk
brew install --cask obs

# 不可用
# todesk \

# 不常用
# arc \
# spotify \
# background-music \
# warp \
# youdaonote \
```

# 特殊版本软件

[[软件推荐]]

# 配置 git

[[Git#git 基本配置]]

# 配置 vim

[[Vim 配置文件]]

# Nvm

[[Nvm]]

# Npm

[[配置]]

# npm global package

```bash
pnpm i -g git-open \
yarn \
serve \
http-server \
pnpm \
nrm \
ts-node
```

> 与 windows 同步 [[Windows系统装机配置指北#npm global package]]

# 配置 iterm2

分屏进入上一个目录

![](http://obsidian.easyhappy.top/avan/20230630210705.png)

# 配置 Alfred

[[Alfred 5]]
