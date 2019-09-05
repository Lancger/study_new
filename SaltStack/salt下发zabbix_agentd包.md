# 一、Centos包
```
#创建zabbix用户和启动程序
salt -N cfd cmd.run "groupadd zabbix && useradd -M -g zabbix -s /sbin/nologin zabbix"

#分组下发
cd /srv/salt/
salt -N cfd cmd.run "/opt/zabbix/init/zabbix_agentd stop"
salt -N cfd cmd.run "mv /opt/zabbix /tmp/zabbix_$$"
salt -N cfd cmd.run "rm -rf /tmp/zabbix_agentd_v4.2.1*"
salt-cp -N cfd zabbix_agentd_v4.2.1.tar.gz /tmp/
salt -N cfd cmd.run "tar -zxvf /tmp/zabbix_agentd_v4.2.1.tar.gz -C /tmp/"
salt -N cfd cmd.run "mv /tmp/zabbix_agentd_v4.2.1 /opt/zabbix"
salt -N cfd cmd.run "chown -R zabbix:zabbix /opt/zabbix"
salt -N cfd cmd.run "/opt/zabbix/init/zabbix_agentd restart"


#批量安装simplejson
salt -N cfd cmd.run "cd /tmp/ && wget --no-check-certificate https://bootstrap.pypa.io/get-pip.py && python get-pip.py && pip install --upgrade pip --trusted-host mirrors.aliyun.com -i https://mirrors.aliyun.com/pypi/simple/ && pip install --upgrade setuptools==30.1.0 && pip install simplejson --trusted-host mirrors.aliyun.com -i https://mirrors.aliyun.com/pypi/simple/"

#自动发现
salt -N cfd cmd.run 'echo "zabbix ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/zabbix'
salt -N cfd cmd.run 'sed -i "s/^Defaults.*.requiretty/#Defaults    requiretty/" /etc/sudoers'
```

# 二、Ubuntu包
```
cat > /srv/salt/zabbix_agentd.conf << \EOF
PidFile=/tmp/zabbix_agentd.pid
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

#分发
cd /srv/salt/
salt -N fd-ubuntu cmd.run "cd /tmp/ && wget -O /tmp/zabbix-release.all.deb http://repo.zabbix.com/zabbix/4.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_4.2-1%2Btrusty_all.deb && dpkg -i zabbix-release.all.deb && apt-get -y install zabbix-agent && apt-get update -y"
salt -N cfd-ubuntu cmd.run 'mkdir -p /etc/zabbix/zabbix_agentd.d/'
salt-cp -N cfd-ubuntu zabbix_agentd.conf /etc/zabbix/zabbix_agentd.conf 
salt -N cfd-ubuntu cmd.run 'chown -R zabbix:zabbix /etc/zabbix/zabbix_agentd.d/'
salt -N cfd-ubuntu cmd.run 'service zabbix-agent restart'
```
