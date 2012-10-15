---
layout: post
title: "[备忘]串号是如何发生的，我的一次串号查找经历"
tags: ruby sinatra rack unicorn
---

## 过程

之前开发的一个 Web 应用，在内测时出现串号，具体情况如下。

串号的症状很典型，某人登录后，再刷新就进入别人的帐号了，也就是两个用户的两次登录共享了同一个 session id cookie, 后登录的用户数据覆盖了前一个用户的登录数据，所以前一个用户在刷新后进入后一个用户的帐户。

这个应用的开发框架是 sinatra, 用 unicorn 做应用服务器，多进程单线程模式。

最开始怀疑是 http cache 出了问题(sinatra 框架也确实没对 cache 做很好的处理，但我们找错了方向)，对 cache 配置做了一些调整，但过了半天又有用户报告串号。

之后在测试服务器加了很多日志，用 ab 软件模拟请求，发现确实服务端会产生同样的 session id。具体原因如下

1. sinatra 框架使用的是缺省的 rack session id 生成算法，这个算法基于 Kernel.rand (现在 rack 已经缺省基于 SecureRandom): http://goo.gl/gEc5W

2. 我们使用的 session storage 是 memcache, 但 memcache storage 在使用新生成 session id 前没有检验 session id 是否存在: (已经找不到当年的代码链接了)， 另，即使检验，也会有竞态问题需要考虑

3. 最关键的是 unicorn 为了提高效率，采用了 fork 的方式来产生工作进程，fork 之后并没有重置 Kernel.rand (unicorn 3.6.1 后已经解决该问题)

问题的解决方式很简单，向 rails 学习, 替换 Rack 的 Session Id generator 即可。

## 教训

* 轻量级框架在很多问题上考虑不够周全，如果你的应用对效率要求不高，尽量用 rails, django 这类的 full-stack 框架。
* 某些重要的信息最好能在 nginx 中纪录，方便排查，比如 session id, request id 等等
* 寻找错误方向，尽量是基于测试而不是基于猜测。在猜测可能是 cache 造成问题之后，我们并没有做试验验证就匆匆发布，结果果然没解决问题。
