lvs实验
##########




实验环境
===========

::

    DR Server
    Hostname: vos1.alv.pub
    Et0-IP: 192.168.105.201
    Eth0:1-VIP: 192.168.105.222 Netmask 255.255.255.255

    Real Server 1
    Hostname: vos2.alv.pub
    Eth0-IP: 192.168.105.202
    lo:0-VIP:192.168.105.222 Netmask 255.255.255.255

    Real Server 2
    Hostname vos3.alv.pub
    Eth0-IP: 192.168.105.203
    Lo:0-IP:192.168.105.222 Netmask 255.255.255.255


DR端的安装配置
=======================

.. code-block:: bash

    yum install ipvsadm -y
    echo 1 > /proc/sys/net/ipv4/ip_forward
    ifconfig eth0:1 192.168.105.222 netmask 255.255.255.255 up
    route add -host 192.168.105.222 dev eth0:1
    [root@vos1 ~]# ipvsadm -A -t 192.168.105.222:80 -s rr
    [root@vos1 ~]# ipvsadm -a -t 192.168.105.222:80 -r 192.168.105.202:80 -g
    [root@vos1 ~]# ipvsadm -a -t 192.168.105.222:80 -r 192.168.105.203:80 -g

执行watch "ipvsadm -Lnc"可以监控连接状态

Real Server 1的安装配置
==============================
.. code-block:: bash

    yum install httpd -y
    echo web1 > /var/www/html/index.htm
    /etc/init.d/httpd start
    ifconfig lo:0 192.168.105.222 netmask 255.255.255.255 up
    route add -host 192.168.105.222 dev lo:0
    echo "1" >/proc/sys/net/ipv4/conf/lo/arp_ignore
    echo "2" >/proc/sys/net/ipv4/conf/lo/arp_announce
    echo "1" >/proc/sys/net/ipv4/conf/all/arp_ignore
    echo "2" >/proc/sys/net/ipv4/conf/all/arp_announce

Real Server 2 的安装配置
=================================
.. code-block:: bash

    yum install httpd -y
    echo web2 > /var/www/html/index.htm
    /etc/init.d/httpd start
    ifconfig lo:0 192.168.105.222 netmask 255.255.255.255 up
    route add -host 192.168.105.222 dev lo:0
    echo "1" >/proc/sys/net/ipv4/conf/lo/arp_ignore
    echo "2" >/proc/sys/net/ipv4/conf/lo/arp_announce
    echo "1" >/proc/sys/net/ipv4/conf/all/arp_ignore
    echo "2" >/proc/sys/net/ipv4/conf/all/arp_announce



客户端测试
==================

可以看到，第一次访问时web2，再访问一次，就变成web1了，再访问，又成了web2,，也就是说，我们访问vos1上的VIP那个IP 192.168.105.222的时候，被转到vos2和vos3上去了，成功实现了轮询。

.. code-block:: bash

    [root@vos4 ~]# curl 192.168.105.222
    web2
    [root@vos4 ~]# curl 192.168.105.222
    web1
    [root@vos4 ~]# curl 192.168.105.222
    web2
    [root@vos4 ~]# curl 192.168.105.222
    web1



lvs默认没有健康检测，不能自动将故障节点删除，所以即使有一个后端节点无法提供服务了，lvs这边也会将请求调度到那个故障节点。