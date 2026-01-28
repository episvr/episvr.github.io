---
title:  Install Debian on ThinkPad-T420
date: 2026-01-28
categories: 
  - Diary
tags: 
  - random_note
description: 我一直想收一台二手笔记本玩玩 Linux，终于下手了，本篇文章是记录这件事的部分流程
mermaid: true
---



我一直想收一台二手笔记本玩玩 Linux，在前前后后纠结了半个学期后正好看到不错的价格，收了一台 ThinkPad-T420，这篇文章就是在这台笔记本上写的。我想回顾一下我倒腾他的短暂的一小段时间，不得不说 Thinkpad 的键盘的真的超级好用。 

![Thinkpad-keyboard](/img/Thinkpad-T420/1.png)



## Why ThinkPad & Why not WSL

首先是为什么不用虚拟机 / WSL 跑来用性能低的二手笔记本。这就要涉及到我的需求：

Need 1： 外带

我需要一台可以随时外带的电脑，因为我的主力机已经常驻 Source 地下室了，我不想来回装卸各种线。
我还希望它耐造一点，因为我的主力机已经快要散架了（应该是屏轴断了但我一直没管）。
最好还能保证尽可能久的续航，因为我不太想到处找电源并背负一个沉重的配件（指电源）。

Need 2： 操作系统

Linux，志在必得

没有什么特殊的原因，馋很久了。
然后我平时喜欢写各种各样的小工具，WSL & 虚拟机的冷启动时间正好我觉得不太舒坦。

然后就是选哪个发行版的问题，因为刻板印象里 debian 超级稳定所以用了。

Need 3：实际需求

轻量级的代码编写
Markdown 笔记编写
读读小说 / 漫画啥的
刷刷网页

## Install Debian

- Task 00：下单

跟卖家协商了一下线下自提的事，但是公共交通通勤时间 1h30min 属实给我整不会了，于是卖家看在我是学生的分上提出均分邮费，我说好耶。没想到邮费💰将近 20, 不嘻嘻。

下单后翻出 U 盘，下载镜像文件进行启动盘的制作。因为有重装系统的经验，这个过程还是很顺利的。

- Task 01：验机

下单后的第二天笔记本就到了，心情激动的把本子取回来了。拆开后的第一感觉是：“厚”。



下图是对比图，左边是Thinkpad T420 右边是 华硕天选4。虽然图片上可能看着差不多实际差了很多。

![Thickpad](/img/Thinkpad-T420/2.jpg)



仔细看看，有很多神秘的远古科技：可拆卸电池、光驱、VGA 接口、miniDP 接口、小红点、触摸板、还有好久没见过的神秘的充电接口。一下子就触发了隐藏 CG，小时候看过我哥用的笔记本，印象中好像是 IBM，也是一模一样的配置。（泪点解析：令人感慨万千）

感慨完了开机，简单查看了一下配置信息无误就直接拿出准备已久的启动盘。

不出意外的是真的出了意外。开机后光驱开始异响，亮灯，然后弹出，然后我塞回去。再异响弹出。初步判断是光驱驱动的问题，结果搜了半天也没有搜索到驱动下载地址，只得作罢…… 吗？在一篇不算古早的帖子我找到了解决办法，就是在 BIOS 从硬件接口上 disable 掉光驱，在搜这篇文章的过程中我还意外得知这个光驱支撑架可以取出来换一块硬盘进去，原来早期的笔记本可拆卸程度这么高的吗。

正好我也要去 BIOS 关闭一些刷系统前的必要启动设置。索性一趟配置完了。接下来就是重头戏了。

- Task 02：要开始了哟

没开始成功，进行了若干次闪退。不过在若干次闪退后，终于莫名其妙的进入了引导安装程序，之后就是点击下一步，直到配置网络的时候，系统无论如何也识别不到 wifi, 但是网卡却没有问题，怎么回事呢？

![strange-switch](/img/Thinkpad-T420/3.jpg)

ThinkPad 的高端设计成功困惑了我十分钟，我没有认真调查，但是我的脑子告诉我大概这是网卡的硬件屏蔽开关，所以把它打开就好。之后就没有什么阻碍了，顺利安装，然后等待机器重启即可。

## Before Custom Config

开机后当然装几个必要的软件了。

- Task 05：Install ibus

随后我遇到的第一个问题就是中文输入法的问题。经过查阅资料我大概得知了我需要安装 ibus/fcitx。然后我发现已经自动预装过了 ibus，这不巧了吗，直接安装 `ibus-pinyin` 然后一个 reboot 就搞定了。

事情当然不会这么简单，重启后确实能输入中文了，但是输入法在 VSCode 的表现却不是很 ok，还有延迟，吞字闪退的问题，这个效果我自然是不满意的。于是连根拔起做了 `sudo apt remove`，准备换 fcitx，仔细一查，fcitx 还分 4 版本和 5 版本，于是怀着 “有新的找新的” 原则准备安装 fcitx5。

- Task 06：Install fcitx5

安装还是很顺利的，然后 VSCode 也能正确的输入中文，但是出现了一个很诡异的问题：系统预装的软件和 VSCode 都能输入中文，但是我自己后来安装的 firefox 和 qq 都不能，我去检索了这个问题，有相当多的人都有这样的问题，我跟着帖子一个一个试，修改环境变量，修改启动参数，换用更低的软件版本，换用 fcixt4…… 都没有解决问题。

最后在一篇 2014 年的帖子里面我看到了翻系统 syslog 的定位方法，原来是之前频繁的装卸 fcitx 和 fcitx5 的框架把依赖的库的版本弄坏了，彻底删除全部的依赖后重启系统，然后重装依赖后搞定。

- Task 07：Beautify

最后就简单配置一下倒腾倒腾软件，下面是成品：

![ThinkMac](/img/Thinkpad-T420/4.png)

![](/img/Thinkpad-T420/5.png)

今天要放的歌的名字还挺神秘的，叫《只々》。（以防你搜不到，它在《空間、事情、時間、事象。》专辑里）

<div class="lyrics-container">
    <div class="song-info">
        <p class="song-title"><svg class="w-6 h-6 text-gray-800 dark:text-white" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" width="24"
                                   height="24" fill="currentColor" viewBox="0 0 24 24">
            <path
                  d="M18.6216 3.21667c.2391.1897.3784.47817.3784.78334V15.6667c0 .0412-.0025.0818-.0073.1218.0048.0698.0073.1404.0073.2115 0 1.6569-1.3431 3-3 3s-3-1.3431-3-3 1.3431-3 3-3c.3506 0 .6872.0602 1 .1707V9.2602l-8 1.8667V18l-.00001.0032C8.99824 19.6586 7.65577 21 6 21c-1.65685 0-3-1.3431-3-3s1.34315-3 3-3c.35064 0 .68722.0602 1 .1707V6.33334c0-.46474.32018-.86823.77277-.97384l9.99953-2.33321c.1486-.03477.3012-.03465.4467-.00201.1427.03202.2783.09532.3964.18752.0021.00162.0041.00324.0062.00487Z" />
            </svg> 只々</p>
    </div>
    <div class="lyrics-container">
  <div class="lyrics-display">
    <div class="lyric-item">
      <div class="original-lyric">今日</div>
      <div class="translated-lyric">今天</div>
    </div>
    <div class="lyric-item">
      <div class="original-lyric">室の窓側</div>
      <div class="translated-lyric">房间的窗边</div>
    </div>
    <div class="lyric-item">
      <div class="original-lyric">空あ 只々 只々</div>
      <div class="translated-lyric">天空 仅仅 只是</div>
    </div>
    <div class="lyric-item">
      <div class="original-lyric">青く 寂びしく</div>
      <div class="translated-lyric">蔚蓝 而又寂寞</div>
    </div>
    <div class="lyric-item">
      <div class="original-lyric">それは美しい</div>
      <div class="translated-lyric">那真是美妙绝伦</div>
    </div>
    <div class="lyric-item">
      <div class="original-lyric">今日</div>
      <div class="translated-lyric">今天</div>
    </div>
    <div class="lyric-item">
      <div class="original-lyric">室の窓側</div>
      <div class="translated-lyric">房间的窗边</div>
    </div>
    <div class="lyric-item">
      <div class="original-lyric">空あ 只々 只々</div>
      <div class="translated-lyric">天空 仅仅 只是</div>
    </div>
    <div class="lyric-item">
      <div class="original-lyric">青く 寂びしく</div>
      <div class="translated-lyric">蔚蓝 而又寂寞</div>
    </div>
    <div class="lyric-item">
      <div class="original-lyric">それは美しい</div>
      <div class="translated-lyric">那真是美妙绝伦</div>
    </div>
  </div>
</div>
</div>
