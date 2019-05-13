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
salt-cp "jys*" limits.conf /etc/security/limits.conf

salt-cp "jys*" 90-nproc.conf /etc/security/limits.d/90-nproc.conf

salt-cp "jys*" def.conf /etc/security/limits.d/def.conf
```

参考资料:

