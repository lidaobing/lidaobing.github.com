---
layout: post
title: "[备忘] 存在依赖的 Debian 包的 pbuilder 编译方法"
tags: debian packaging pbuilder
---

某些时候，需要上传的包的依赖包还未出现在 debian 官方仓库中，比如你需要上传 input-pad 1.0.0-1 和 ibus-input-pad 0.1.4-1, 后者依赖于前者, input-pad 可以正常地安装和上传, 但 ibus-input-pad 却会出现编译失败，因为对应的 input-pad 1.0.0-1 还未出现在官方 Debian 仓库中, 这种情况的处理方法如下所示

* 先按照 ﻿在 Ubuntu 下构建 Debian 打包环境  这篇配置好环境
* 安装 apache, 激活 userdir, 创建自己的 debian 目录

      $ sudo apt-get install apache2 ﻿mini-dinstall
      $ sudo a2enmod userdir
      $ sudo /etc/init.d/apache2 restart
      $ mkdir -p ~/public_html/debian

* 访问 http://127.0.0.1/~lidb/debian/ 确认能正常显示
* 察看 /etc/dput.cf, 保证有如下一段

      [local]
      method      = local
      incoming    = ~/public_html/debian/mini-dinstall/incoming
      run_dinstall    = 0
      post_upload_command = /usr/bin/mini-dinstall --batch

* 创建 $HOME/.mini-dinstall.conf, 内容如下所示

      [DEFAULT]
      archivedir = ~/public_html/debian/
      archive_style = flat
      architectures = all, amd64, i386

* 安装 input-pad 1.0.0-1 到本地目录

      $ cd /var/cache/pbuilder-dist/sid_result
      $ dput local input-pad_1.0.0-1_amd64.changes

* 在 $HOME/.pbuilderrc 里添加如何一行

      OTHERMIRROR="deb http://localhost/~lidb/debian unstable/"

* 运行 "pbuilder-sid update"
* 运行 "﻿pbuilder-sid build ibus-input-pad_0.1.4-1.dsc"

备注

* 每次向本地上传包后都要重新运行 pbuilder-sid update
