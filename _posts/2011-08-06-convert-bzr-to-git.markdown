---
layout: post
title: "[备忘] 把 bzr 仓库转为 git 仓库"
tags: bzr git
---

    $ sudo apt-get install bzr-fastimport git-core
    $ bzr clone lp:~lidaobing/iptux/iptux
    $ cd iptux/
    $ git init
    $ bzr fast-export . | git fast-import
    $ gitk --all
    $ rm -r .bzr
    $ git reset HEAD
