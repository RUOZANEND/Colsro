---
title: Android使用 Termux 搭建web服务并实现外网访问
categories:
  - Tech
tags:
  - Termux
  - Android
  - WEB
date: 2020-04-13 15:33:00
updated: 2020-04-13 15:33:00
thumbnail: /imgs/acmp/cover.jpg
pslug: acmp
---
相信各位家里面应该都有闲置的Android设备，反正也是闲置，不如利用起来搭建一个小网站。

<!--more-->

**文章已失效，仅供参考**

emmmm，说出来你们可能不信，这篇文章在typecho迁移至hugo时丢失了一大半内容，现在连参考价值都没了(-̩̩̩-̩̩̩-̩̩̩-̩̩̩-̩̩̩___-̩̩̩-̩̩̩-̩̩̩-̩̩̩-̩̩̩)

## 前言

termux是什么：

> Termux是一个Android下一个高级的终端模拟器, 开源且不需要root, 支持apt管理软件包，十分方便安装软件包, 完美支持Python, PHP, Ruby, Go, Nodejs, MySQL等。

## 一、优化termux使用

因为一些原因，termux自带的软件源在国内的速度并不理想，所以我们先使用以下命令修改为清华大学提供的软件源。

```
sed -i 's@^\\(deb.\*stable main\\)$@#\\1\\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/termux-packages-24 stable main@' $PREFIX/etc/apt/sources.list  
sed -i 's@^\\(deb.\*games stable\\)$@#\\1\\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/game-packages-24 games stable@' $PREFIX/etc/apt/sources.list.d/game.list  
sed -i 's@^\\(deb.\*science stable\\)$@#\\1\\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/science-packages-24 science stable@' $PREFIX/etc/apt/sources.list.d/science.list  
pkg up  
```

输入： `termux-setup-storage` 获取并授予存储权限

输入以下内容修复快捷键：

```
echo "extra-keys = \[\['ESC','TAB','CTRL','ALT','-','/','~',':',';'\],\['\[','!','PGUP','HOME','UP','END','PGDN','\\"','\]'\],\['{','<','(','LEFT','DOWN','RIGHT',')','>','}'\]\]" >> ~/.termux/termux.properties  
```

可选操作：

安装zsh：

```
sh -c "$(curl -fsSL https://gitee.com/idkzr/termux-ohmyzsh/raw/master/install.sh)"  
```

## 二、安装Caddy MariaDB PHP

Caddy是一个易于使用的通用web服务器。

MariaDB数据库管理系统是MySQL的一个分支，主要由开源社区在维护，采用GPL授权许可 MariaDB的目的是完全兼容MySQL，包括API和命令行，使之能轻松成为MySQL的代替品。

使用 `bash <(curl https://colsro.com/shell/acmp.sh)`即可一键安装

什么？不想使用一键命令？

在Termux中依次执行

```
pkg i wget vim mariadb php php-fpm -y  
  
curl https://getcaddy.com | bash -s personal  
```

## 三、配置MySql数据库

输入 `mysqld` 启动数据库，启动完成后,这个会话就一直存活,类似与debug调试一样,只有新建会话才可以操作。

新建一个termux会话

输入`mysql`直接进入mariadb数据库，输入：

```mysql
set password for "root"
```
