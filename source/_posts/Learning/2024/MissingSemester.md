---
title: Missing Semester学习总结（上）
date: 2024-09-05
categories: 
  - Learning
tags: 
  - coding
description: 传奇课程missing semester，威大，无需多验
---

观前注意：这不是一份完备的学习笔记，更多的是epi自己的总结回顾之类的，如果完全没有接触该课程的请移步[B站](https://www.bilibili.com/video/BV1uc411N7eK/)，当然你可以寻找其他的资源。

> 记得社团面试(2024.4)的时候我在看missing semester，直到现在(2024.9)我还在看missing semester，执行力弱爆了，踩着暑假的尾巴终于整个看了一遍，把之前断断续续的记忆拼接起来了（

讲义：[The Missing Semester of Your CS Education](https://missing-semester-cn.github.io)



## P1: Shell入门

- 命令：date，echo，which，pwd，cd，. ，..，ls，~，-，mv，cp，rm，rmdir，mkdir，man，<，>，cat，>>，|，tail，sudo，#，tee，find
- 选项（flag & option）：--help
- 单引号与双引号，转义，环境变量，路径变量，相对/绝对路径，文件和路径的rwx权限，输入输出流，root

> **fun stuffs**
>
> `sudo rm -rf /*`   Do as Super User, remove all ~~rubbish files~~ recursively forcefully.
>
> `man`, what ~~[the flags and options]~~ can I say!

## P2: Shell编程

- source，true，false，for ，grep，if，-eq，diff，cp，touch，convert，env，history
- 保留关键字：`$0 $1~$9 $? $_ !! $date $# $$ $@ $(CMD)  ` 
- 空格，字符串(单双引号的区别)， 逻辑短路，分号，命令替换，标准错误流，正则表达式，花括号的笛卡尔积式扩展
- 涉及到的一些工具：tldr，fd，locate，ripgrep，fzf，tree，broot

> **fun stuffs**
>
> 1. ~~不知道为什么~~放弃了之前装好的虚拟机不用，去自己重装了一个，然后不停在各种多手教程中瞎折腾。最后统统打回原样，只保留了zsh和vim的相关配置
> 2. 由于没有设置里没点桥接网卡，尝试ping了10.0.2.15
> 3. 迷惑，适应，然后效仿老师的奇妙口音
> 4. foo，bar被称为**Metasyntactic variable**，并没有什么实际意思
> 5. `!!` means BANG BANG

## P3: vim入门

- 多模式，关闭、保存，插入，多开
- vim的~~不~~普通模式
  - 移动`h j k l` `w b e` `0 $ ^` `Ctrl-U Ctrl-D` `G gg` `H L M`
  - 查找 f F t T /
  - 增行 o O
  - 删除 d c x
  - 替代 r
  - 撤销与反撤销 u Ctrl-R
  - 复制与粘贴 y p
  - 可视模式 v V Ctrl-V
  - 反转大小写 ~
  - 计数
  - 修饰符 i a
  - 括号跳转 %
  - 重复上一个命令 .

> **fun stuffs**
>
> 1. 好舒服的发音
> 2. [vim-adventures](https://vim-adventures.com/)好玩捏

## P4: 数据整理(Data Wrangling)

- journalctl，ssh，less，sed，sort，uniq，awk，xarg，paste，bc
- `. * ? () ^ $ \ -E g`，贪婪匹配，捕获组
- R语言，gnuplot，ffmpeg
- 正则表达式调试器

> **fun stuffs**
>
> 1. woc，没看懂再看一遍
> 2. woc，没看懂再看一遍
> 3. woc，没看懂再看一遍

## P5: 命令行环境

### 进程管理：Job Control

- `^C`，`^\`，`^Z` ，kill，fg，bg，jobs，&，nohup
- 信号

### 终端复用：Terminal Multiplexer

- 会话 >> 窗口 >> 面板
- 快捷键`^B`+`cmd`
  - 会话 `new`，`ls`，`d`，`a`，`-s`，`-t`
  - 窗口 `c`，`num`，`p`，`n`，`,`，`w` 
  - 面板`"` ，`%`，`z`， `[`，`space`，` direction`

### 配置文件：Dotfiles

- alias，ln

### 远端设备：Remote Machines

- ssh，ssh密钥

> **fun stuffs**
>
> 1. 针不辍针不辍，折腾配置文件最后删除重装针不辍
> 2. 有一次敲 vim ./vimrc，空白一片给我吓坏了，后来pwd发现自己在Desktop（乐



## 小结（JUST MORE FUNSTUFF）：

- 回想起了大一上学的一门~~那时候还就觉得很水的~~课
- vim-adventures的Level3之后要付费，[too sad](https://web.archive.org/web/20201013182647/https://github.com/wattry/vim-adventures)
- 现在依然折服于P4后半段的信息量，好有实力
- 推荐去做课后题，真有趣吧

![](https://cdn.jsdelivr.net/gh/episvr/IMG_Cloud//tmp.jpg)
