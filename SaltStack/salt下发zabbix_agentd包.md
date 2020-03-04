# 一、Centos包
```
#创建zabbix用户和启动程序
salt -N group-01 cmd.run "groupadd zabbix && useradd -M -g zabbix -s /sbin/nologin zabbix"

#分组下发
cd /srv/salt/
salt -N group-01 cmd.run "/opt/zabbix/init/zabbix_agentd stop"
salt -N group-01 cmd.run "mv /opt/zabbix /usr/local/src/zabbix_$$"
salt -N group-01 cmd.run "rm -rf /usr/local/src/zabbix_agentd_v4.2.1*"
salt-cp -N group-01 zabbix_agentd_v4.2.1.tar.gz /usr/local/src/
salt -N group-01 cmd.run "tar -zxvf /usr/local/src/zabbix_agentd_v4.2.1.tar.gz -C /usr/local/src/"
salt -N group-01 cmd.run "mv /usr/local/src/zabbix_agentd_v4.2.1 /opt/zabbix"
salt -N group-01 cmd.run "chown -R zabbix:zabbix /opt/zabbix"
salt -N group-01 cmd.run "/opt/zabbix/init/zabbix_agentd restart"

#批量安装simplejson
salt -N cfd cmd.run "cd /tmp/ && wget -N --no-check-certificate https://bootstrap.pypa.io/get-pip.py && python get-pip.py && pip install --upgrade pip --trusted-host mirrors.aliyun.com -i https://mirrors.aliyun.com/pypi/simple/ && pip install --upgrade setuptools==30.1.0 && pip install simplejson --trusted-host mirrors.aliyun.com -i https://mirrors.aliyun.com/pypi/simple/"

#还需注意自动发现目录的权限
salt -N cfd cmd.run 'chmod -R 755 /data0/opt/'

#自动发现
salt -N cfd cmd.run 'echo "zabbix ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/zabbix'
salt -N cfd cmd.run 'echo "Defaults:zabbix !requiretty" >> /etc/sudoers.d/zabbix'
salt -N cfd cmd.run 'sed -i "s/^Defaults.*.requiretty/#Defaults    requiretty/" /etc/sudoers'
salt -N cfd cmd.run 'cat /etc/sudoers | grep requiretty'
salt -N cfd cmd.run 'cat /etc/sudoers.d/zabbix'
```

# 二、Ubuntu包

```
#查看安装软件包
dpkg -l | grep 'zabbix'
ii  zabbix-agent                          1:4.2.6-1+trusty                           amd64        Zabbix network monitoring solution - agent
ii  zabbix-release                        4.2-1+trusty                               all          Zabbix official repository configuration

#卸载(dpkg -P 彻底卸载，包括配置文件等。)
dpkg -P zabbix-agent
dpkg -P zabbix-release

#启动脚本
/lib/systemd/system/zabbix-agent.service

#设置开机启动zabbix-agent
sudo update-rc.d zabbix-agent enable
```

```
cat > /srv/salt/zabbix_agentd.conf << \EOF
PidFile=/run/zabbix/zabbix_agentd.pid
LogFile=/tmp/zabbix_agentd.log
LogFileSize=0
DebugLevel=2
Server=192.168.52.133
ServerActive=192.168.52.133
EnableRemoteCommands=1
UnsafeUserParameters=1
HostnameItem=system.run[echo $(hostname)]
HostMetadataItem=system.uname
Include=/etc/zabbix/zabbix_agentd.d/*.conf
EOF

sed -i 's/Server.*/Server=192.168.52.133/g' /srv/salt/zabbix_agentd.conf
sed -i 's/ServerActive.*/ServerActive=192.168.52.133/g' /srv/salt/zabbix_agentd.conf


salt-cp -N aws001 zabbix_agentd.conf /opt/zabbix/etc/zabbix_agentd.conf
或者
salt -N aws001 cmd.run "sed -i 's/Server.*/Server=192.168.52.133/g' /srv/salt/zabbix_agentd.conf"
salt -N aws001 cmd.run "sed -i 's/ServerActive.*/ServerActive=192.168.52.133/g' /srv/salt/zabbix_agentd.conf"

#分发
cd /srv/salt/
salt -N cfd-ubuntu cmd.run "cd /tmp/ && wget -O /tmp/zabbix-release.all.deb http://repo.zabbix.com/zabbix/4.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_4.2-1%2Btrusty_all.deb && dpkg -i zabbix-release.all.deb && apt-get -y install zabbix-agent && apt-get update -y"
salt -N cfd-ubuntu cmd.run 'mkdir -p /etc/zabbix/zabbix_agentd.d/'
salt-cp -N cfd-ubuntu zabbix_agentd.conf /etc/zabbix/zabbix_agentd.conf 
salt -N cfd-ubuntu cmd.run 'chown -R zabbix:zabbix /etc/zabbix/zabbix_agentd.d/'
salt -N cfd-ubuntu cmd.run 'service zabbix-agent stop'
salt -N cfd-ubuntu cmd.run 'service zabbix-agent start'
```

# 三、批量重启
```
salt -N centos-all cmd.run "/opt/zabbix/init/zabbix_agentd restart"
salt -N ubuntu-all cmd.run "service zabbix-agent restart"
```
