# 一、Ansible-A机器，ssh代理配置

jenkins账户设置的免key登陆

```
[root@Ansible-A]# pwd
/opt/jenkins/.ssh

[root@Ansible-A]# cat config 
Host 10.33.99.*
  ProxyCommand ssh -W %h:%p 130.35.35.13
  IdentityFile /opt/jenkins/.ssh/ansible_id_rsa

Host 130.35.35.13
  Hostname proxy.ansible.com
  User root
  IdentityFile /opt/jenkins/.ssh/ansible_id_rsa
```

#  二、Ansible-B代理机器，ssh代理配置

root账户设置的免key登陆

```

[root@Ansible-B]# pwd
/root/.ssh

[root@Ansible-B]# cat config 

Host 130.35.35.13
  Hostname twww921
  IdentityFile /root/.ssh/id_rsa
  User root
  ControlMaster auto
  ControlPath /tmp/.ssh/ansibe-%r@%h:%p
  ControlPersist 5m
```

# 三、ssh直接验证
```
ssh直接验证测试

ssh root@10.33.99.19 -i  /opt/ansible/.auth/id_rsa 

Last login: Thu May  9 16:31:29 2019 from 10.33.99.228

[root@www259 ~]# 
```

# 三、
