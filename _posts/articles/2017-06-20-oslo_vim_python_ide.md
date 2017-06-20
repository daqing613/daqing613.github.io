---
layout: post
title: "Vim Python IDE 开发环境"
excerpt: "文档描述的是如何使用Vundle搭建Vim Python开发环境，提供工作效率。" 
categories: articles
tags: ['vim', 'python', 'IDE']
comments: true
share: true
image:
  feature: so-simple-sample-image-6.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---


已经使用Vim作为python开发工作编辑器很长一段时间了， 觉得很有必要整理一下；即可以在新的机器上快速搭建开发环境， 也可以帮助有需要的人员减少入门的痛苦。 

# 安装

* Debian/Ubuntu 平台

  ```
  sudo apt-get install python vim exuberant-ctags git

  sudo pip install dbgp pep8 flake8 pyflakes isort
  ```
  
* RedHat/CentOS 平台

  `sudo yum install python vim ctags git`

> Note: 你已经对Vim有一定的了解， 知道它的基本使用。

----
# Vundle

这里我们使用Vundle作为插件管理器；用它来管理插件的安装和更新。 


## 安装Vundle

`git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim`

## 配置插件

```
set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" 下面是一些插件例子

" Python mode (indentation, doc, refactor, lints, code checking, motion and
" operators, highlighting, run and ipdb breakpoints)
Plugin 'klen/python-mode'
" Better autocompletion
Plugin 'Shougo/neocomplete.vim'

" Python-mode ------------------------------

" don't use linter, we use syntastic for that
let g:pymode_lint_on_write = 0
let g:pymode_lint_signs = 0
" don't fold python code on open
let g:pymode_folding = 0
" don't load rope by default. Change to 1 to use rope
let g:pymode_rope = 0
" open definitions on same window, and custom mappings for definitions and
" occurrences
let g:pymode_rope_goto_definition_bind = ',d'
let g:pymode_rope_goto_definition_cmd = 'e'
nmap ,D :tab split<CR>:PymodePython rope.goto()<CR>
nmap ,o :RopeFindOccurrences<CR>

" NeoComplete ------------------------------

let g:neocomplete#enable_at_startup = 1

```

## 安装插件

运行Vim然后执行 `:PluginInstall`

或者
命令行执行： `vim +PluginInstall +qall`


> https://github.com/VundleVim/Vundle.vim

----

# vim plugins

这里有很多有用的plugins, 可以按照实际情况进行定制。 

## NerdTree

一个树形的浏览文件系统插件。 

> https://github.com/scrooloose/nerdtree

## Ctags


生成tags文件

`ctags -R  /source-path/`

func直接跳转
```
Ctrl-]  jump to fun
Ctrl-o  jump back 
```

vim“找到 tag: 1/4 或更多” 其他定义的查看方法
```
:tselect 显示列表

:tn和:tp 显示后一个tag和前一个tag
```
ctags for centos 7 
> http://rpm.pbone.net/index.php3/stat/4/idpl/29070714/dir/centos_7/com/ctags-5.8-13.el7.x86_64.rpm.html 

## NeoComplete

Neocomplcache的下一代completion框架

> https://github.com/Shougo/neocomplete.vim

## PyMode

PyLint, Rope, Pydoc, breakpoints。 

> https://github.com/python-mode/python-mode

## TagBar

在windows显示tags。

> https://github.com/majutsushi/tagbar
