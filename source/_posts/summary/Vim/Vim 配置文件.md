---
title: Vim 配置文件
date: 2025-10-01T17:18:54.000Z
tags:
  - 汇总
  - Vim
  - 配置
categories:
  - 汇总
  - Vim
uuid: 6e7ec6ff-09d4-45c7-9fe5-c3b718223ab1
---

# 1、相关配置文件

~/.ideavimrc
~/.vimrc

```bash
# 安装插件管理
git clone git@github.com:Layouwen/Vundle.git ~/.vim/bundle/Vundle.vim
```

# 2、配置

**通用**

```
" ================================ base ================================
:set clipboard=unnamedplus,unnamed
:set ignorecase
:set hlsearch
:set incsearch
:set nu rnu

let mapleader = " "

noremap H ^
noremap L g_
noremap J 5j
noremap K 5k

nnoremap <leader>ns :nohlsearch<CR>
nnoremap <Tab> :tabn<CR>
nnoremap <C-Tab> :tabp<CR>

map <leader>to :NERDTreeFocus<CR>
map <leader>ts :NERDTreeClose<CR>
" ================================ base ================================
```

**.vimrc**

```
" ================================ vimrc ================================
set nocompatible              " be iMproved, required
filetype off                  " required

set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

Plugin 'VundleVim/Vundle.vim'
Plugin 'git@github.com:easymotion/vim-easymotion.git'
Plugin 'git@github.com:justinmk/vim-sneak.git'
Plugin 'git@github.com:preservim/nerdtree.git'
Plugin 'github/copilot.vim'

call vundle#end()            " required
filetype plugin indent on    " required

map s s
map S S

nmap f <Plug>Sneak_s
nmap F <Plug>Sneak_S
" ================================ vimrc ================================
```

**.ideavimrc**

```
" ================================ ideavim ================================
" :actionlist 可以查看所有 actions

set easymotion
set surround
set NERDTree
set multiple-cursors

map <leader>fd <Action>(ReformatCode)
map <leader>fs <Action>(Javascript.Linters.EsLint.Fix)
map f <Action>(AceAction)
map F <Action>(AceTargetAction)

map <leader>j <Action>(EditorJoinLines)
map <leader>rn <Action>(RenameElement)
map <leader>sof <Action>(SelectIn)
" ================================ ideavim ================================
```

# Obsidian

.obsidain.vimrc

```
set clipboard=unnamed

unmap <Space>

map H ^
map L $
map J 5j
map K 5k

exmap back obcommand app:go-back
nmap <C-o> :back
exmap forward obcommand app:go-forward
nmap <C-i> :forward

exmap dailyNotes obcommand daily-notes
nmap <Space>nn :dailyNotes
exmap dailyNotesPrev obcommand daily-notes:goto-prev
nmap <Space>np :dailyNotesPrev

exmap deleteFile obcommand app:delete-file
nmap <Space>df :deleteFile

exmap wiki surround [[ ]]
map [[ :wike

exmap foldAll obcommand editor:fold-all
nmap zM :foldAll
exmap unfoldAll obcommand editor:unfold-all
nmap zR :unfoldAll
exmap toggleFold obcommand editor:toggle-fold
nmap za :toggleFold
```

# 键表

<k0> - <k9> 小键盘 0 到 9

<S-...> Shift＋键

<C-...> Control＋键

<M-...> Alt＋键 或 meta＋键

<A-...> 同 <M-...>

<Esc> Escape 键

<Up> 光标上移键

<Space> 插入空格

<Tab> 插入Tab

<CR> 等于<Enter>
