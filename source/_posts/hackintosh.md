---
title: hackintosh
date: 2019-04-10 18:26:08
tags: operating system
---

## Hackintosh

Hackintosh, 与Macintosh相对；黑与白。

### 引导方式

通电开机, 主板上ROM芯片的BIOS(Basic Input/Output System), 开始运行.

- 第一阶段：BIOS
  1. 硬件自检. POST(Power-On Self-Test). catch err: 不同含义的蜂鸣.
  2. BIOS把控制权转交给下一阶段的启动程序. Boot Sequence
- 第二阶段：主引导记录
  1. 计算机读取排在第一位的储存设备的第一个扇区, 也就是读取最前面的512个字节, 即主引导记录MBR(Master boot record).
  2. 主引导记录的结构: 机器码, 分区表, 主引导记录签名(0x55和0xAA)
  3. 分区表: 64个字节，里面又分成四项，每项16个字节, 由6个部分组成.
- 第三阶段：硬盘启动
- 第四阶段：操作系统
  - 以老的Linux系统为例, 先载入/boot目录下面的kernel. /sbin/init. pid进程编号为1
  - 新的Linux系统, Systemd 代替了init

- BIOS和UEFI
  1. BIOS是引导MBR, 把它读出来交给CPU执行，做MBR做想做的事.
  2. UEFI是引导是启动到文件, 查找磁盘里的\efi\boot\\\*.efi文件, 启动这个可执行程序, 让这程序做想做的事.


- 分类
  1. BIOS+MBR分区
  2. UEFI+GPT分区