---
layout: post
title: "debian/watch 如何排除非稳定版"
tags: debian packaging
---

﻿某些软件，比如 glib, gtk, 2.12.0 是稳定版, 2.13.1 是非稳定版，如果只想打包稳定版，又不想 debian/watch 因为检测到版本更高的非稳定版报警的话，可以用正则表达式排除非稳定版，范例如下所示

      version=3
      http://www.spice-space.org/download/releases/spice-protocol-(\d+\.\d*[02468]\.\d+)\.tar\.bz2
