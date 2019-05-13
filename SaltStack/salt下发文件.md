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
```

参考资料:

