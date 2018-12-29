# 一、定时任务配合程序开关ansible通道
```
[root@www ~]# crontab -l
*/5 * * * * /sbin/iptables -Z

##fOR ANSIBLE SECURITY
0 22 * * 1,2,3,4,5,6,7 /opt/.action/encrypt.sh >/dev/null 2>&1
0 8 * * 1,2,3,4,5 /opt/.action/decrypt.sh >/dev/null 2>&1
```
# 二、加锁通道
```
[root@www ~]# cat /opt/.action/encrypt.sh
#!/bin/sh

ansible-vault encrypt /opt/ansible/.auth/id_rsa
ansible-vault encrypt /root/.ssh/id_rsa
```
# 二、解锁通道
```
[root@www ~]# cat /opt/.action/decrypt.sh
#!/bin/sh

ansible-vault decrypt /opt/ansible/.auth/id_rsa
ansible-vault decrypt /root/.ssh/id_rsa

chown jenkins:jenkins /opt/ansible/.auth/id_rsa
```
