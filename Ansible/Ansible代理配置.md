
```
/opt/jenkins/.ssh

ansible机器，ssh代理配置
$ cat config 

Host 10.33.99.*
  ProxyCommand ssh -W %h:%p 130.35.35.13
  IdentityFile /opt/jenkins/.ssh/ansible_id_rsa


Host 130.35.35.13
  Hostname proxy.ansible.com
  User root
  IdentityFile /opt/jenkins/.ssh/ansible_id_rsa
  
ansible代理机器，ssh代理配置

[root@twww921 .ssh]# pwd
/root/.ssh
[root@twww921 .ssh]# cat config 
Host 130.35.35.13
  Hostname twww921
  IdentityFile /root/.ssh/id_rsa
  User root
  ControlMaster auto
  ControlPath /tmp/.ssh/ansibe-%r@%h:%p
  ControlPersist 5m

```

# 二、ssh直接验证
```
ssh直接验证测试

ssh root@10.33.99.19 -i  /opt/ansible/.auth/id_rsa 

Last login: Thu May  9 16:31:29 2019 from 10.33.99.228

[root@www259 ~]# 
```

# 三、
