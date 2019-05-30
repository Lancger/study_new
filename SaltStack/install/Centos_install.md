# 一、简介

    Saltstack 比 Puppet 出来晚几年，是基于Python 开发的，也是基于 C/S 架构，服务端 master 和客户端 minions ；Saltstack 和 Puppet 很像，可以说 Saltstatck 整合了 Puppet 和 Chef 的功能，更加强大，更适合大规模批量管理服务器，并且它比 Puppet 更容易配置。
    
    三大功能： 远程命令执行，配置管理（服务，文件，cron，用户，组），云管理。
    
    支持系统：大多数都支持，windows 上不支持安装 master。

# 二、安装
```
0) 准备工作

准备两台机器，这两台机器都关闭 selinux，清空 iptables 规则并保存。

master：192.168.56.11
slaver：192.168.56.12

0) 修改hosts

sudo tee /etc/hosts << 'EOF'
192.168.56.11  master.test.com
192.168.56.12  slaver.test.com
EOF

或 EOF 加 \ 表示不解析变量

cat > /etc/hosts << \EOF       
192.168.56.11  master.test.com
192.168.56.12  slaver.test.com
EOF

1）服务端安装

yum install -y epel-release
yum install -y salt-master salt-minion

2）客户端安装

yum install -y epel-release
yum install -y salt-minion

```

# 三、配置
```
1）服务端和客户端都要配置 master

以下两种方式都可以，选择其中一种即可
# vim /etc/salt/minion

# master改为服务端的主机名
master: master.test.com

# master改为服务端的IP
master: 192.168.56.11


分别修改三台机器minion文件中的的id为自己的主机名
# vim /etc/salt/minion +78
id: slaver.test.com

最终修改
sudo tee /etc/salt/minion << 'EOF'
master: 182.168.56.11
id: slaver.test.com
EOF


2）启动saltstack服务

服务端

systemctl enable salt-master
systemctl enable salt-minion
systemctl restart salt-master
systemctl restart salt-minion

客户端
systemctl enable salt-minion
systemctl restart salt-minion
```

# 四、配置认证
```
1）在服务端上操作

salt-key -a  slaver.test.com
salt-key -a  master.test.com
salt-key

说明：
-a ：accept ，
-A：accept-all，
-d：delete，
-D：delete-all。
可以使用 salt-key 命令查看到已经签名的客户端。此时我们在客户端的 /etc/salt/pki/minion 目录下面会多出一个minion_master.pub 文件。


2）测试验证

示例1： 

salt '*' test.ping                  //检测通讯是否正常，也可以指定其中一个 'slaver.test.com'

示例2:  

salt '*' cmd.run   'df -h'         //远程执行命令

说明： 这里的 * 必须是在 master 上已经被接受过的客户端，可以通过 salt-key 查到，通常是我们已经设定的 id 值。关于这部分内容，它支持通配、列表以及正则。 比如两台客户端 web10、web11， 那我们可以写成  salt 'web*'    salt 'web1[02]'  salt -L 'web10,web11'   salt -E 'web(10|11)' 等形式，使用列表，即多个机器用逗号分隔，而且需要加-L，使用正则必须要带-E选项。 它还支持 grains 和 pillar，分别加 -G 和 -I 选项，下面会介绍到。
```


命令行
```
yum install -y epel-release
yum install -y salt-minion

sudo tee /etc/salt/minion << 'EOF'
master: 192.168.56.11
id: slaver.test.com
EOF

systemctl enable salt-minion
systemctl restart salt-minion
```
