---
title: Linux常用命令学习与实战
date: 2024-06-14
categories: 
  - Learning
tags: 
  - coding
description: shell命令真是...
---



🚩**对应《华为Openeuler开源操作系统》的 实战任务4~任务8**

------

# 任务四

## 不同的提示符

超级用户root 的 Shell 提示符是`#`
普通用户 的 Shell 提示符是美元货币符`$`

## 用左低右高的斜杠`/`来表示 Linux 文件系统的根目录

注意，在 **UNIX/Linux** 中表示根目录时，使用的是左低右高的斜杠`/`
而在 **Windows** 中表示根目录时，使用的是左高右低的斜杠`\`

## tree 的命令选项

`-u`选项表示显示用户名或者 （UID）
`-d`选项表示只显示目录（directory）
`-L`选项表示显示目录的层级（layer）

## 注释

`#`后面的内容是Shell的注释

## 绝对路径 与 相对路径

绝对路径是从Linux文件系统目录树的根`/`开始，经过一些目录，最后到达目标目录或者目标文件
当前的工作路径也称**当前目录**`.`
两个点号`..`用来表示当前目录的上一级目录，或者称为**父目录**
使用 Linux 命令`pwd`可以打印当前的工作路径
如果使用了用来表示当前目录的`.`和用来表示当前目录的父目录的`..`，那么就说采用了**相对路径**来命名目录和文件。

<!--more-->

## 文件(目录)名的通配符有两个（正则表达式检索）

星号`*` 表示0个或者多个字符。
问号`?`表示一个字符。

## 用户的主目录

Linux用户首次登录系统后所处的目录称为主目录 (Home Directory)
这个目录一般位于/home/UserName

- 例如用户 omm，它的主目录在 /home/omm 下。

Linux 上的用户一般只对自己的主目录有完全的读写控制权，这种安排可以保证 Linux 操作系统中用户数据的安全。

## ls 命令 ***list***

**列出目录和文件的信息**

ls命令的语法如下:`ls [OPTION] … [FILE] …`

- 没有任何选项的 ls 命令
  当前目录文件名与子文件名
- ls命令的 -a 选项（*all*）当前目录下所有的文件名和目录名（包括隐藏文件,隐藏目录）
- ls命令的-l选项（*long*）,显示文件或目录的详细信息
- 组合命令-al等
- ls命令的-F选项（*flag*）

```markdown
    * 表示可执行文件
    / 表示目录
    @ 表示文件符号链接
    | 表示文件是管道文件
    = 表示文件是套接字
    > 表示进程间的通信设备
```

- ls命令的-d选项（*directory*）只显示目录本身的信息
- ls命令的-R选项 （*recursive*）递归地显示目录及其所有子目录中的文件信息
- ls命令的-i选项（*index*）显示文件或者目录的索引节点号

## mkdir命令 ***make directory***

**在文件系统目录树上创建新的目录**

命令语法：`mkdir [options] DirectoryName`

- 没有选项的 mkdir 命令
- mkdir命令的-p选项 (*parents*) 创建多层目录

## rmdir命令 ***remove directory***

删除一个空目录

## du命令 ***disk using***

查看磁盘存储空间占用情况

语法:`du [options] DirectoryName`

- -s选项(*size*): 只显示目录的总计大小，不显示该目录下子目录的大小
- -k选项: 以 KB 为单位显示大小
- -m选项: 以 MB 为单位显示大小
- -h选项(*human*) :以人类可读的方式显示大小

## touch命令 ***touch***

作用1: 变更最后修改时间，达到重新编译的目的

作用2: 创建新的空文件

## cp命令 ***copy***

**复制文件和目录**

语法：`cp [options] SourceFile DestinationFile`

- 没有选项的cp命令: 只能用来复制文件
- cp命令的-r选项 (recursive): 复制一个目录及子目录下的所有文件
- cp命令的-p选项（preserve）: 保留原文件的文件属性

## ln 命令 ***link***

**创建文件和目录的链接**

文件和目录的链接有两种:硬链接 和 软链接

语法：→`ln [options]file-existed file-linked`

- 硬链接

   

  ```
  ln file-existed file-linked
  ```

  - 硬链接不能跨越文件系统。因为文件名不是文件的属性，因此可以在同一个文件系统下让某个文件只存储一个副本，但是该副本具有与原文件不同的名字。这可以通过使用 ln 命令为文件创建硬链接来实现。
  - 索引节点一致，只是文件名不同

- 软链接

  ```
  ln -s file-existed file-linked
  ```

  - 当需要在两个不同的文件系统之间创建链接时，只能创建软链接
  - 索引节点不同，是两个文件

## mv命令 ***move***

语法`mv OldfileName NewFileName`

## rm命令 ***remove***

语法 `rm [option] file-or-directory-list`

- 没有选项的rm命令 仅能删除目录
- rm 命令的-r选项 (*recursive*) 删除非空目录
- rm命令的-i选项 (*interactive*) 交互式确认
- rm命令的-f选项 (*force*) 屏蔽提醒

## cat命令 ***concatenate***

**显示小于一页文件的内容**

语法`cat FileList`

新文件`cat >fileB <<EOF`

## more命令 ***more***

**查看多页文件的内容**

```
more FileName
 Enter 下一行
 b     上一页
 空格  下一页
 q     结束命令
```

## head命令 ***head***

**显示文件前几行的内容**

```
head [option] file
```

- 没有选项的head命令
  - 默认前10行
- head命令的-n选项
  - `head -n num`表示显示前num行

## tail命令 ***tail***

语法 `tail [option] file`

- 没有选项的tail命令
- tail命令的-n选项

## file命令 ***file***

**查看文件的类型**

## grep命令 ***global regular expression***

**搜索文件中的某一字符串**

语法→`grep [option] pattern FileList`

- 没有选项的grep命令
- grep命令的-n选项
  - 显示行号
- grep命令的-v选项
  - 反向搜索（不包含搜索模式的行）

## diff命令 ***different***

## find命令 ***find***

**搜索指定名字的文件**

```
find Pathname -option [-print] [-exec command{} ;]`
`find Pathname -option [-print] [-ok command{} ;]
```

- find 命令的-name选项
  - 查找指定名字的文件
- find命令的-exec选项
  - 查找指定名字的文件执行linux命令
- find命令的-ok选项
  - 在-exec选项的基础上，加入确认环节

## wc命令 ***Word Count***

**统计文件的行数，单词数，字符数**

语法 `wc [options] file`

- [options]的常用项

```diff
-l  显示行数
-w  显示单词数
-c  字符数
```

## whereis命令 ***where is***

**在特定目录查找符合特定条件的文件**

语法`whereis [options] [-B DirList][-M DirList][-S DirList] Filelist`

- [options]的常用项

```diff
-b                                          只查找二进制文件
-f                                           不显示文件名的前的路径名称
-m                                         仅查找man手册文件
-s                                           仅查找源代码文件
-u                                           反向查找
-B DirList                               在设置的目录下查找二进制文件
-M DirList                              在设置的目录下查找man手册文件
-S DirList                               在设置的目录下查找源代码文件        
```

## which命令 ***which***

**查找指定程序的文件在哪个文件下**

语法→`which Command`

## locate 命令 # locate

**根据名字来查找文件**

语法→`locate [options] FileName`

- [options]的常用选项

```diff
-h                                          帮助 # help 
-V                                          版本信息  # version 
-q                                          安静模式，不显示错误信息 # quiet 
-i                                           忽略大小写  # ignore 
-c                                          仅输出指定的数量 # count 
-l num                                  最多输出num个条目
-n num                                 最多显示n个输出
-d DBPATH                           使用DBPATH指定的数据库 
```

## sort 命令

**对文件内容进行排序**
`sort [options] FileName`

```diff
-n : 数字值排序
-M ：识别三字符月份，按时间排序
-r ：反向排序
```

------

# 任务五 文件和目录权限管理

**属主，属组，其他用户**
**r（read）4 ，w（write）2， x（execute）1**
eg：rwxr-----对应的是740

## chmod 命令 ***change mode***

```
chmod [options] Mode file-or directory
```

eg：`chmod 750 filename`

## chown 命令 ***change owner***

```
chown [options] owner file-or directory
```

------

# 任务六 进程管理

**父进程，子进程，天生进程，兄弟进程，孤儿进程，僵尸进程**

## ps 命令：

`ps [options]` **（process status）**

- 无参ps
- -ef命令 *every full*
- -elf命令 *every long full*
- -aux命令

## pstree 命令

**打印家族树信息**

- -p
- -u
- -pu

## kill 命令

```
kill [options] PID
```

- -l
- PID
- -9 PID
- -HUP PID

## pgrep 命令

**搜索进程**

```
pgrep [options] <pattern>
```

- -o
- -n
- -l
- -P
- -g
- -t
- -u

## killall 命令