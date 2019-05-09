# 一、Ansible-A机器，ssh代理配置

jenkins账户设置的免key登陆

```
[jenkins@Ansible-A]# pwd
/opt/jenkins/.ssh


# 匹配内网机器10.33.99.*段的机器通过跳转机130.35.35.13跳转

[jenkins@Ansible-A]# cat config 
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

# 三、代理验证
```
#在Ansible-B机器验证

ansible test01 -m ping --private-key /opt/ansible/.auth/id_rsa
test01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}


#在Ansible-A机器验证(注意需要在/etc/hosts下绑定主机)

vim /etc/hosts
10.33.99.19 test01

vim /etc/ansible/hosts
[web]
test01  ansible_ssh_host=10.33.99.19  ansible_ssh_user=root

ansible test01 -m ping --private-key /opt/ansible/.auth/id_rsa
test01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}


#ssh直接验证测试

ssh root@10.33.99.19 -i  /opt/ansible/.auth/id_rsa 

Last login: Thu May  9 16:31:29 2019 from 10.33.99.228

[root@www259 ~]# 
```


参考文档：

https://hoxis.github.io/ansible-jump-server.html

