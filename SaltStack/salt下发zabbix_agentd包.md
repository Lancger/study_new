```
#分组下发
cd /srv/salt/
salt -N cfd cmd.run "mv /opt/zabbix /tmp/zabbix_$$"
salt -N cfd cmd.run "rm -rf /tmp/zabbix_agentd_v4.2.1*"
salt-cp -N cfd zabbix_agentd_v4.2.1.tar.gz /tmp/
salt -N cfd cmd.run "tar -zxvf /tmp/zabbix_agentd_v4.2.1.tar.gz -C /tmp/"
salt -N cfd cmd.run "mv /tmp/zabbix_agentd_v4.2.1 /opt/zabbix"

#创建zabbix用户和启动程序
salt -N cfd cmd.run "groupadd zabbix && useradd -M -g zabbix -s /sbin/nologin zabbix"
salt -N cfd cmd.run "chown -R zabbix:zabbix /opt/zabbix"
salt -N cfd cmd.run "/opt/zabbix/init/zabbix_agentd restart"

#批量安装simplejson
salt -N cfd cmd.run "cd /tmp/ && wget --no-check-certificate https://bootstrap.pypa.io/get-pip.py && python get-pip.py && pip install --upgrade pip --trusted-host mirrors.aliyun.com -i https://mirrors.aliyun.com/pypi/simple/ && pip install --upgrade setuptools==30.1.0 && pip install simplejson --trusted-host mirrors.aliyun.com -i https://mirrors.aliyun.com/pypi/simple/"

#自动发现
salt -N cfd cmd.run 'echo "zabbix ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/zabbix'
salt -N cfd cmd.run 'sed -i "s/^Defaults.*.requiretty/#Defaults    requiretty/" /etc/sudoers'
```
