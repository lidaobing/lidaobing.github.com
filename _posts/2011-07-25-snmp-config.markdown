---
layout: post
title: "[备忘] SNMP 配置"
tags: linux snmp
---

## 修改 `/etc/default/snmpd` (适用于Debian/Ubuntu)

把 127.0.0.1 改为 0.0.0.0

## 创建一个 v2 版的只读用户

    # /etc/snmp/snmpd.conf
    # rocommunity 通信密钥 允许IP
    rocommunity foo 1.2.3.4
    rocommunity foo 5.6.7.8

## 重启 snmpd

    $ sudo /etc/init.d/snmpd restart

## 测试

    $ snmpwalk -v 2c -c <通信密钥> <IP或域名> .1.3.6.1.4.1.2021.11.50.0
