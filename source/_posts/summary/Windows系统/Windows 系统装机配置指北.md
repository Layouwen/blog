---
title: Windows 系统装机配置指北
date: 2023-01-23 15:22:00
tags:
  - 汇总
  - windows
categories:
  - 汇总
  - Windows系统
---

# Scoop

安装 scoop 包管理

> 不在推荐改目录

```powershell
# 修改目錄
# [environment]::setEnvironmentVariable('SCOOP','d:\software\scoop','User')
# $env:SCOOP='d:\software\scoop'
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
```

---

添加 bucket 源

```powershell
scoop bucket add extras
scoop bucket add main
scoop bucket add versions
scoop bucket add nonportable
```

安装命令行工具

```powershell
scoop install `
main/vim

# 暂时不使用
# main/nvm `
# pnpm `
```

安装软件

```powershell
scoop install `
extras/postman `
extras/mqttx `
versions/snipaste-beta `
extras/sourcetree `
extras/telegram `
extras/qq `
extras/discord `
extras/vscode `
extras/jetbrains-toolbox `
extras/apifox `
extras/soapui `
extras/firefox `
extras/googlechrome `
extras/picgo `
nonportable/grammarly-np `
extras/mongosh `
extras/powertoys `

# 暫時不使用
# extras/spotify `
# extras/wechat `
# extras/obsidian `
# extras/wpsoffice `
```

# 特殊版本

[[软件推荐]]

# 相关配置

[[相关配置]]

# 配置 git

[[开发小锦囊#git 基本配置]]

# Nvm

[[Nvm]]

# fnm

## powershell

```bash
# 安装 fnm
winget install Schniz.fnm
# 创建配置文件
if (-not (Test-Path $profile)) { New-Item $profile -Force }
# 打开文件目录
start <目录>
```

添加到配置文件中

```bash
fnm env --use-on-cd --shell powershell | Out-String | Invoke-Expression
```

## wsl

```bash
curl -fsSL https://fnm.vercel.app/install | zsh
```

# 配置 vim

[[Vim配置文件]]

# npm global package

```bash
npm i -g `
git-open `
yarn `
serve `
http-server `
pnpm `
nrm `
ts-node
```

> 与 mac 同步 [[Mac系统装机配置指北#Pnpm global package]]