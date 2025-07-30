---
title: Mac 系统装机配置指北
date: 2024-12-11 15:22:00
tags:
  - 汇总
  - mac
categories:
  - 汇总
  - Mac系统
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

下载 brew

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

如果需要使用清华国内源，先设置环境变量，在 `brew update`

设置按键时长

![](http://obsidian.easyhappy.top/avan/20230301005456.png)

设置三脂拖拽

![](http://obsidian.easyhappy.top/obsidian/20230617133802.png)

开启轻点触发

![](http://obsidian.easyhappy.top/obsidian/20230617133842.png)

设置 F1-F12 为默认键

![](http://obsidian.easyhappy.top/obsidian/20230617204437.png)

设置显示秒

![](http://obsidian.easyhappy.top/obsidian/20230618082251.png)

## 无法按住持续连按

### 关闭 ApplePressAndHoldEnabled

```bash
defaults write com.jetbrains.WebStorm ApplePressAndHoldEnabled -bool false # webstorm
defaults write com.jetbrains.intellij ApplePressAndHoldEnabled -bool false # idea
defaults write com.runningwithcrayons.Alfred ApplePressAndHoldEnabled -bool false # Alfred

defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false              # For VS Code
defaults write com.microsoft.VSCodeInsiders ApplePressAndHoldEnabled -bool false      # For VS Code Insider
defaults write com.visualstudio.code.oss ApplePressAndHoldEnabled -bool false         # For VS Codium
defaults write com.microsoft.VSCodeExploration ApplePressAndHoldEnabled -bool false   # For VS Codium Exploration users

defaults write -g ApplePressAndHoldEnabled -bool false # 全局设置

defaults delete -g ApplePressAndHoldEnabled                                           # If necessary, reset global default
```

# brew相关

```bash
brew install \
cowsay \
zplug \
trash \
neofetch \
scc \
duf \
diff-so-fancy \
lolcat \
fzf \
figlet \
tmux \
graphicsmagick \
cmatrix \
ctop

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
brew install --cask \
iterm2 \
bob \
mqttx \
firefox \
postman \
nutstore \
soapui \
jetbrains-toolbox \
iina \
adrive \
baidunetdisk \
apifox \
wechatwebdevtools \
visual-studio-code \
ticktick \
snipaste \
wechat \
qq \
telegram \
sourcetree \
sunloginclient \
wechatwork \
feishu \
discord \
tencent-lemon \
neteasemusic \
picgo \
macdown \
1password \
drawio \
microsoft-edge \
mongodb-compass \
chatbox \
steam \
google-chrome \
notion \
notion-calendar \
tencent-meeting \
josm \
hbuilderx \
font-jetbrains-mono \
sublime-text \
thunder \
obsidian

# 新版
brew install --cask \
docker \
proxyman \
todesk \
dingtalk \
arc \
obs \

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

[[Vim配置文件]]

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