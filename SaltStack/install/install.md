# 一、简介

    Saltstack 比 Puppet 出来晚几年，是基于Python 开发的，也是基于 C/S 架构，服务端 master 和客户端 minions ；Saltstack 和 Puppet 很像，可以说 Saltstatck 整合了 Puppet 和 Chef 的功能，更加强大，更适合大规模批量管理服务器，并且它比 Puppet 更容易配置。
    三大功能： 远程命令执行，配置管理（服务，文件，cron，用户，组），云管理。
    支持系统：大多数都支持，windows 上不支持安装 master。

# 二、安装
```
1）服务端安装

[iyunv@master ~]# yum install -y epel-release
[iyunv@master ~]# yum install -y salt-master salt-minion

2）客户端安装

[iyunv@slaver ~]# yum install -y epel-release
[iyunv@slaver ~]# yum install -y salt-minion

```
