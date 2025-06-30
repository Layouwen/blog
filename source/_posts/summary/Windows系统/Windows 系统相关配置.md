---
title: Windows 系统相关配置
date: 2022-01-23 15:22:00
tags:
  - 汇总
  - windows
categories:
  - 汇总
  - Windows系统
---

# powershell 7

安装

```powershell
winget search Microsoft.PowerShell
winget install --id Microsoft.Powershell.Preview --source winget
```

在 store 安装 terminal preview

将 powershell 7(pwsh) 设置为默认

# 键盘配置

![image](https://pic1.zhimg.com/80/v2-4ccb797ee77be7ab8ac6d82adc041650_1440w.png)

# PowerToys

![image](https://pic4.zhimg.com/80/v2-70373d883052a1b490cff1efe175c49b_1440w.png)

![image](https://pica.zhimg.com/80/v2-8228e137955ffe0ae189d95375c158b0_1440w.png)

# 使用 WSL 方案

## Terminal

### 设置默认终端为 wsl 的 ubuntu

直接在 terminal 的 setting json 中找到对应 uuid 填入

### 在终端开启代理

```bash
cd ~
touch openproxy.sh
vim openproxy.sh
```

填入下面内容

```bash
export http_proxy="http://127.0.0.1:10887"
export https_proxy="http://127.0.0.1:10887"

echo "already open proxy with 127.0.0.1:10887
```

> 127.0.0.1 如果是在虚拟机，需要填入vpn开启的 ip 地址

```bash
touch closeproxy.sh
vim closeproxy.sh
```

填入

```bash
unset http_proxy
unset https_proxy

echo "already close proxy"
```

添加别名，在 `.bashrc` `.zshrc` 你使用的终端添加

```bash
vim .bashrc
```

加入

```bash
alias openproxy="source ~/.command/openproxy.sh"
alias closeproxy="source ~/.command/closeproxy.sh"
```

也可以直接

**.zshrc**

```bash
alias openproxy="export http_proxy='http://192.168.0.103:21882'; export https_proxy='http://192.168.0.103:21882'"
alias closeproxy="unset http_proxy; unset https_proxy"
```

使用方法

```bash
# 开启
openproxy
# 关闭
closeproxy
```

### 安装 zsh

```bash
sudo apt install zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

zplun 用于安装插件

```bash
curl -sL --proto-redir -all,https https://raw.githubusercontent.com/zplug/installer/master/installer.zsh | zsh
```

```bash
source ~/.zplug/init.zsh

zplug "zsh-users/zsh-syntax-highlighting", defer:2

if ! zplug check --verbose; then
    printf "Install? [y/N]: "
    if read -q; then
        echo; zplug install
    fi
fi

zplug load --verbose
```

编辑 .zshrc 文件，参考 

> sudo apt install cowsay

### 安装 nvm

```zsh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | zsh
```

# 使用 Git Bash 方案

安装 Windows 包管理工具，打开 powershell `非管理员模式`

```bash
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
irm get.scoop.sh | iex
```

安装常用包

```bash
# lazygit
scoop bucket add extras
scoop install lazygit

# neofetch
scoop install neofetch
```