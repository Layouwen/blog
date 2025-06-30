---
title: 快速上手Vim
date: 2020-08-14 23:34
tags:
  - 博客
  - vim
categories:
  - 博客
  - vim
---

## 简单操作

i 在光标前输入
a 在光标后输入
shift+i 当前行最前面输入
shift+a 当前行最后面输入
o 在光标的下一行输入内容
shift+o 在光标上一行输入内容
s 删除光标的字符并进入输入模式
x 删除当前光标下的字符
d 数字 放向键 自定义在什么方向删除多少个字符/行
dd 删除整行
p 粘贴
y 数字 方向键 自定义在什么方向复制多少个字符/行
w 进入下一个单词的开头
c 删除当前光标的字符
c 数字 方向键 自定义在什么方向删除多少个字符/行
c w 删除整个单词
c i w 删除你光标所在的单词
c i 字符 删除自定义内部的所有内容
d i 字符 删除（复制）自定义内部的所有内容
y i 字符 复制自定义内部的所有内容
f 查找模式
f 字符 查找自定义字符，并到指定位置
d f 字符 删除（复制）到自定义的字符位置
y f 字符 复制到自定义的字符位置
/ 搜索
ESC 返回指令模式
:w 保存
:q 退出
:wq 保存退出
:split 上下分屏
:vsplit 左右分屏
0 回到最开头

## 配置vimrc文件

在~根目录创建.vim文件夹

```vim
mkdir .vim
cd .vim
vim vimrc
```

```vim
noremap 当前键位 映射键位
map 键位 指令
```

```vim
let mapleader=" "
syntax on
set number
set relativenumber
set cursorline
set wrap
set showcmd
set wildmenu
set hlsearch
exec "nohlsearch"
set incsearch
set ignorecase

noremap = <LEADER><CR> :nohlsearch<CR>

map s <nop>
map S :w<CR>
map Q :q<CR>
map R :source $MYVIMRC<CR>
```

syntax on 打开语法高亮
set number 打开行号
set relativenumber 打开高亮行号
set cursorline 高亮当前行线
set wrap 换行
set showcmd 显示输入的命令
set wildmenu 显示命令菜单
set hlsearch 高亮你搜索的结果
exec "nohlsearch" 每次进入vim都自动清空搜索结果
set incsearch 边输入边搜索
set ignorecase 忽略大小写搜索
set smartcase 正常忽略大小写，全大写时精确搜索
noremap = <LEADER><CR> :nohlsearch<CR> 设置空格+回车清空搜索结果
s 取消映射
S 保存
Q 退出
R 刷新配置文件

> 所有使用set的属性，可以在属性名前面加个no关闭
> hjkl是vim中的移动键，如果想快速移动可以自定义按键映射`行数+方向键`