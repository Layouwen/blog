---
title: 个人的 Obsidian 数据同步和文章预览方案
date: 2026-05-03 02:11:36
tags:
  - 博客
  - electron
  - 同部工具
categories:
  - 博客
  - 工具
---

原本想着是用 node 脚本实现 Obsidian 的文章复制到 blog 项目中, 在 deploy 到 github page 上. 脑子一热, 想着多此一举的用 electron 可视化页面操作. 干脆 Vibe Coding 一下, 发现效果似乎不错.

> PS: 本篇文章就是用 electron 写的 Avan Toolkit 实现一键同步, 具体内容等完善后上传到 github 上面在更新这个文章


```mermaid
graph TD
    A[设备A: Obsidian 编辑] -->|坚果云同步| C(坚果云云端)
    B[设备B: Obsidian 编辑] -->|坚果云同步| C
    C -->|同步到本地| D[各设备本地 Obsidian 仓库<br>（整个 Vault）]
    
    D -->|GitHub 插件<br>提交整个仓库| H1[(GitHub 私有仓库<br>Obsidian 备份)]
    H1 -.->|新设备拉取恢复| D
    
    D --> E[Obsidian 内「blog」文件夹存放 Hexo 文章]
    E -->|Electron 一键复制| F[本地 Hexo 项目<br>（含 /source, themes, _config.yml）]
    
    F -->|git push 备份| H2[(GitHub 仓库<br>Hexo 源码备份)]
    H2 -.->|新设备克隆| F
    
    F -->|hexo deploy| G[GitHub Pages 预览]
```
