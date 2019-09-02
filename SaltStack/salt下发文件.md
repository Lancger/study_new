# 一、salt-master端配置

```
[root@ip-172-31-30-95 /etc/salt]# cat /etc/salt/master
 file_recv: True
 file_recv_max_size: 100000
 file_roots:
   base:
     - /srv/salt/
 pillar_roots:
  base:
    - /srv/pillar
```

# 二、下发指定文件

```
#执行命令
salt-cp "jys*" limits.conf /etc/security/limits.conf
salt-cp "jys*" 90-nproc.conf /etc/security/limits.d/90-nproc.conf
salt-cp "jys*" def.conf /etc/security/limits.d/def.conf

#执行结果
{'jys001': {'/etc/security/limits.d/def.conf': True},
 'jys002': {'/etc/security/limits.d/def.conf': True},
 'jys003': {'/etc/security/limits.d/def.conf': True},
 'jys004': {'/etc/security/limits.d/def.conf': True},
 'jys005': {'/etc/security/limits.d/def.conf': True},
 'jys006': {'/etc/security/limits.d/def.conf': True}}
 
#检查
cd /etc/security/


#下发防火墙配置
salt-cp -N centos7-1rd iptables /etc/sysconfig/iptables
salt -N centos7-1rd cmd.run "systemctl restart iptables.service"

salt-cp -N centos7-2rd iptables /etc/sysconfig/iptables
salt -N centos7-2rd cmd.run "systemctl restart iptables.service"

salt-cp -N centos7-3rd iptables /etc/sysconfig/iptables
salt -N centos7-3rd cmd.run "systemctl restart iptables.service"


salt -N centos7-3rd cmd.run "systemctl restart supervisord"
salt -N centos7-3rd test.ping


#下发代码配置

单台下发
cd /srv/salt/
salt 'coinearn-io-07-redis-mq-new' cmd.run "mv /opt/cn/ /tmp/cn-2019-bak"
salt-cp 'coinearn-io-07-redis-mq-new' cn-2rd.tar.gz /tmp/
salt 'coinearn-io-07-redis-mq-new' cmd.run "tar -zxvf /tmp/cn-2rd.tar.gz -C /opt/"

salt 'coinearn-io-07-redis-mq-new' cmd.run "mv /opt/config-project/ /tmp/config-project-2019-bak"
salt-cp 'coinearn-io-07-redis-mq-new' config-project-2rd.tar.gz /tmp/
salt 'coinearn-io-07-redis-mq-new' cmd.run "tar -zxvf /tmp/config-project-2rd.tar.gz -C /opt/"


#分组下发
cd /srv/salt/
salt -N centos7-2rd cmd.run "mv /opt/cn/ /tmp/cn-2019-bak"
salt-cp -N centos7-2rd cn-2rd.tar.gz /tmp/
salt -N centos7-2rd cmd.run "tar -zxvf /tmp/cn-2rd.tar.gz -C /opt/"

salt -N centos7-2rd cmd.run "mv /opt/config-project/ /tmp/config-project-2019-bak"
salt-cp -N centos7-2rd config-project-2rd.tar.gz /tmp/
salt -N centos7-2rd cmd.run "tar -zxvf /tmp/config-project-2rd.tar.gz -C /opt/"



#下发配置
salt-cp -N centos7-2rd server_openapi.xml /data0/opt/tomcat8_8085_openapi/conf/server.xml
salt-cp -N centos7-2rd server_inner.xml /data0/opt/tomcat8_8083_inner/conf/server.xml
salt-cp -N centos7-2rd server_match.xml /data0/opt/tomcat8_8084_match/conf/server.xml



salt -N centos7-2rd cmd.run "grep acceptCount /data0/opt/tomcat8_8085_openapi/conf/server.xml" 
salt -N centos7-2rd cmd.run "grep acceptCount /data0/opt/tomcat8_8083_inner/conf/server.xml" 


#批量修改时区
salt -N centos7-1rd cmd.run "date -R"
salt -N centos7-1rd cmd.run "/bin/cp -rf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime"

salt -N centos7-2rd cmd.run "date -R"
salt -N centos7-2rd cmd.run "/bin/cp -rf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime"

salt -N centos7-3rd cmd.run "date -R"
salt -N centos7-3rd cmd.run "/bin/cp -rf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime"

salt -N centos7-4rd cmd.run "date -R"
salt -N centos7-4rd cmd.run "/bin/cp -rf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime"

#针对所有的centos系统
salt -N centos-all cmd.run "date -R"

#针对所有的ubuntu系统
salt -N ubuntu-all cmd.run "date -R"
salt -N ubuntu-all cmd.run "ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime"
```


# 三、批量修改密码
```
#修改密码

Centos服务器：

salt -E "fhex-one-0[5-9]|fhex-one-10" cmd.run 'echo "Kda18*s2311KLC}wXd1"|passwd --stdin root'

Ubuntu服务器：

salt "fhex-one-0[1-4]" cmd.run 'echo root:"Kda18*s2311KLC}wXd1" | chpasswd'

```

# 四、安全加固
```
cat > /srv/salt/hosts.deny << \EOF
#
# hosts.deny    This file contains access rules which are used to
#               deny connections to network services that either use
#               the tcp_wrappers library or that have been
#               started through a tcp_wrappers-enabled xinetd.
#
#               The rules in this file can also be set up in
#               /etc/hosts.allow with a 'deny' option instead.
#
#               See 'man 5 hosts_options' and 'man 5 hosts_access'
#               for information on rule syntax.
#               See 'man tcpd' for information on tcp_wrappers
#
sshd:all:deny
EOF

cat > /srv/salt/hosts.allow << \EOF
#
# hosts.allow   This file contains access rules which are used to
#               allow or deny connections to network services that
#               either use the tcp_wrappers library or that have been
#               started through a tcp_wrappers-enabled xinetd.
#
#               See 'man 5 hosts_options' and 'man 5 hosts_access'
#               for information on rule syntax.
#               See 'man tcpd' for information on tcp_wrappers
#
sshd:192.168.52.0/255.255.255.0
sshd:139.180.20.37,19.10.20.37
EOF

cat > /srv/salt/sshrc << \EOF
/bin/sh  /usr/local/bin/ssh_login_monitor.sh
EOF

cd /srv/salt/
salt-cp -N buy hosts.allow /etc/hosts.allow
salt-cp -N buy hosts.deny /etc/hosts.deny
salt-cp -N buy sshrc /etc/ssh/sshrc
salt-cp -N buy ssh_login_monitor_3rd.sh /usr/local/bin/ssh_login_monitor.sh
```

参考资料:

