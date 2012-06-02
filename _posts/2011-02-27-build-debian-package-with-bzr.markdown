---
layout: post
title: "[备忘] 使用 bzr 仓库时的常用打包命令"
tags: debian packaging bzr
---

* 安装软件

      $ sudo apt-get install bzr-builddeb

* 下载源码包

      $ ﻿bzr clone lp:~lidaobing/ibus/ibus-hangul

* 更新上游包

      $ cd ibus-hangul
      $ uscan --force-download

* 合并到 bzr 仓库

      $ bzr merge-upstream ../ibus-hangul-1.3.1.tar.gz --version 1.3.1
      $ bzr commit

* 查看 bzr log

      $ bzr log -n0 | less

* 编译并安装

      $ debuild -i.bzr
      $ debc
      $ sudo debi

* 调整 changelog

      $ dch

* 准备源码包

      $ debuild -i.bzr -S -sa

* 用 pbuilder 测试源码包

      $ cd ..
      $ sudo pbuilder build ibus-hangul_1.3.1-1.dsc

* 打 tag, 提交代码

      $ bzr tag debian-1.3.1-1
      $ bzr push lp:~lidaobing/ibus/ibus-hangul
