---
layout: post
title: "Deploy snmpd on CentOS for jiankongbao"
tags: centos snmpd jiankongbao snmp
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
* if you have iptables

      $ sudo iptables -I INPUT 1 -p udp -s 60.195.252.107 --dport 161 -j ACCEPT
      $ sudo iptables -I INPUT 1 -p udp -s 60.195.252.110 --dport 161 -j ACCEPT
      $ sudo cp /etc/sysconfig/iptables /etc/sysconfig/iptables.bak
      $ sudo iptables-save | sudo tee /etc/sysconfig/iptables

* verify everything is ok

      $ netstat -lpnu | grep 161
      (No info could be read for "-p": geteuid()=509 but you should be root.)
      udp        0      0 0.0.0.0:161                 0.0.0.0:*                               -
      $ chkconfig --list snmpd
      snmpd           0:off 1:off 2:on  3:on  4:on  5:on  6:off


