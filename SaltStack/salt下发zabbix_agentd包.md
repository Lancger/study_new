```
#分组下发
cd /srv/salt/
salt -N cfd cmd.run "mv /opt/zabbix /tmp/zabbix_$$"
salt-cp -N cfd zabbix.tar.gz /tmp/
salt -N cfd cmd.run "tar -zxvf /tmp/zabbix.tar.gz -C /opt/"
```
