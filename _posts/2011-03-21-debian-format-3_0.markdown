---
layout: post
title: "[备忘] Debian format 3.0"
tags: debian packaging quilt
---

* 加入新文件: debian/source/format, 内容为

      3.0 (quilt)

* 设定环境变量 (可以加到 $HOME/.bashrc 中)

      export QUILT_PATCHES=debian/patches

* 运行如下的命令, 创建第一个补丁

      $ quilt new binutils-gold.patch # 新建补丁，名字叫 binutils-gold.patch
      $ quilt add CMakeLists.txt # 这个补丁需要修改 CMakeLists.txt, 所以把 CMakeLists.txt 加入列表
      $ vi CMakeLists.txt
      ...
      $ quilt refresh # 把更改写入补丁文件
      $ quilt pop     # 从工作区去除补丁

* 检验 debian/patches 目录，确认如下两个文件存在

      binutils-gold.patch
      series

* 用 debuild 测试 (最好安装 lintian 2.2.18, 否则 lintian 会给出一些警告)

* 测试成功后，根据 [DEP3](http://dep.debian.net/deps/dep3/) 写 debian/patches/binutils-gold.patch 这个补丁的元信息 (下面是 fcitx 一个补丁的范例)

      Description: fix FTBFS with binutils-gold
      Forwarded: http://code.google.com/p/fcitx/issues/detail?id=159
      Bug-Debian: http://bugs.debian.org/554282
      Author: LI Daobing <lidaobing@debian.org>
      Last-Update: 2010-02-22

      Index: fcitx/src/Makefile.am
      ===================================================================
      --- fcitx.orig/src/Makefile.am    2010-02-22 20:29:47.000000000 +0800
      +++ fcitx/src/Makefile.am    2010-02-22 20:38:56.000000000 +0800
      @@ -37,4 +37,4 @@
               ImeRemote.c

       fcitx_LDADD = ../lib/libXimd.a
      -fcitx_LDFLAGS = -lXpm -lXtst -lpthread
      +fcitx_LDFLAGS = -lXpm -lXtst -lpthread -ldl -lXext


* 完成，可以提交了。
