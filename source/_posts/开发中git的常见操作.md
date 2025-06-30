---
title: 开发中git的常见操作
date: 2020-08-04 10:28
tags:
  - 博客
  - git
  - 前端
categories:
  - 博客
  - 前端
---

## 设置了HTTPS需要切换为SSH

```bash
git remote rm origin
git remote add origin 你的git的ssh地址
git push -u origin master
```

## 同时上传多个git仓库

绑定对应仓库
```bash
git remote add gitee 你的git的ssh地址
git push -u gitee master
git remote add github 你的git的ssh地址
git push -u github master
```

上传对仓库
```bash
git push github master
git push gitee master
```

## 创建及切换分支

```bash
git branch 分支名
git checkout 分支名
```

## 合并分支

```bash
git check master
git merge 需要合并的分支
git push 源名 master
```

## 其他指令

```bash
更新远程分支列表
git remote update origin --prune

查看所有分支
git branch -a

删除远程分支
git push origin ---delete 删除的分支名

删除本地分支
git branch -d 删除的分支名
```