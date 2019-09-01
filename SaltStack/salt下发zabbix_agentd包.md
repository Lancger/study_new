```
#分组下发
cd /srv/salt/
salt -N cfd cmd.run "mv /opt/zabbix /tmp/zabbix_$$"
salt-cp -N cfd zabbix_agentd_v4.2.1.tar.gz /tmp/
salt -N cfd cmd.run "tar -zxvf /tmp/zabbix_agentd_v4.2.1.tar.gz -C /opt/"
salt -N cfd cmd.run "mv /tmp/zabbix_agentd_v4.2.1 /opt/zabbix"

#创建zabbix用户和启动程序
salt -N cfd cmd.run "groupadd zabbix && useradd -M -g zabbix -s /sbin/nologin zabbix"
salt -N cfd cmd.run "chown -R zabbix:zabbix /opt/zabbix"
salt -N cfd cmd.run "/opt/zabbix/init/zabbix_agentd restart"
```
