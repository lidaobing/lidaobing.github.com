---
layout: post
title: "Deploy snmpd on CentOS for jiankongbao"
tags: centos snmpd jiankongbao
---

* install net-snmp

      $ sudo yum install net-snmp

* edit `/etc/snmp/snmpd.conf`

      # /etc/snmp/snmpd.conf
      # rocommunity key IP
      rocommunity jiankongbao 60.195.252.107
      rocommunity jiankongbao 60.195.252.110

* run `sudo /etc/init.d/snmpd restart`
* run `sudo chkconfig snmpd on`
