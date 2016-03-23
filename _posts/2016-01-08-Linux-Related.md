---
published: true
author: Charles
layout: post
title:  "Linux札记"
date:   2016-01-08 9:30
categories: Linux
---

#### Sublime安装

#### Sublime 安装
官网下载 [deb][1] 包，使用命令 dpkg 相关命令安装。

dpkg是debian package的缩写，为”Debian“ 操作系统 专门开发的套件管理系统，用于软件的安装，更新和移除。

|    dpkg -i package.deb   |         安装包         |
|:------------------------:|:----------------------:|
|      dpkg -r package     |         删除包         |
|      dpkg -P package     | 删除包（包括配置文件） |
|      dpkg -L package     |  列出与该包关联的文件  |
|      dpkg -l package     |     显示该包的版本     |
| dpkg –unpack package.deb |    解开 deb 包的内容   |
|      dpkg -S keyword     |    搜索所属的包内容    |
|          dpkg -l         |   列出当前已安装的包   |
|    dpkg -c package.deb   |    列出 deb 包的内容   |
|  dpkg –configure package |         配置包         |


  [1]: https://www.sublimetext.com/3

