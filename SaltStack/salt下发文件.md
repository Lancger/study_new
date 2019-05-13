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

参考资料:

