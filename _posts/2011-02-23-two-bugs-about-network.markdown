---
layout: post
title: "两个bug"
tags: bug wireshark tcpdump curl
---

## bug0

一个 flash Web 应用，在 Windows 下 IE, Chrome 浏览器下正常, Linux 下 Firefox, Chrome 浏览器下正常, 只有在 Windows 下的 Firefox 不正常, 服务端不返回 200, 改返回 412 了。

原因很简单, Windows 下 Firefox 发送的 HTTP 头是 "Content-length: 1234", 其他所有客户端发送的都是 "Content-Length: 1234", 根据 HTTP 标准\[1\], 这两个都合法, 但是服务器端没考虑 HTTP 头 Field Name 大小写不敏感的问题, 对 Windows 下的 Firefox 认为没发送 Content-Length 头，返回 412 错误

\[1\] [RFC 2616 Section 4.2](http://tools.ietf.org/html/rfc2616#section-4.2): "Field names are case-insensitive."

## bug1

一个 douban 的 OAuth 客户端, 在测试机上正常, 但在正式环境下在 request_token 那步返回 400, OAuth gem 抱怨授权失败。

原因也很简单, 服务器时间不对, 慢了 10 分钟，douban 要求时间戳误差在 300 秒以内。

## 总结

罗列这些 bug 意义不大, 关键在于如何找到 bug 的原因。现在的网络服务也越来越多，大部分都提供了基于 HTTP 的 API , 但大部分客户端在使用时都是使用 SDK, 而不是直接构造 HTTP 请求。出现问题时, 不管是单步跟踪还是修改 SDK 加入日志都很麻烦，而且效果也不好。

对我来说，最好用的工具就是 wireshark, tcpdump 和 curl。 curl 用于构造请求, 这个比用浏览器来构造请求方便很多。wireshark 用于监控网络请求, 比如 bug0 的寻找方法就很简单，先用所以浏览器都再现一遍问题，用 wireshark 拿到所有的请求头，比较请求头中的差异，对于有差异的部分, 用 curl 模拟请求，观察服务器响应情况，很快就能发现 "Content-Length" 有问题，跟服务端开发人员确认一下，果然是这个问题。

对于产品环境，开 wireshark 很不方便, 一般就用 tcpdump 来代替, 比如 "sudo tcpdump -n -s 512 -w 1.log port 80", 然后把 1.log 传回本地，用 wireshark 分析即可。bug1 就是用此方法拿到 HTTP 请求数据, 然后跟本地成功的请求对比后发现时间戳差距太大才发现原因的。

PS1: 当然，象 DNS, 时间不准，时区错误这些都是服务器常出现的问题, 一个负责任的运维不会让你陷入这些恶心的陷阱, 但是你总有运气不好的时候。

PS2: 其实 douban 在 request_token 失败时会提供真实的原因，不过他提供的是一个完整的 html 页面, 既不是 application/json, 也不是 text/plain, 如果你 tcpdump 窗口只开 512 的话，根本看不到这些信息的, 所以窗口开到 4096 也是有必要的，只是日志文件会变大很多
