# 一、Ansible介绍及安装部署

      #备份当前的yum源
      mv /etc/yum.repos.d /etc/yum.repos.d_backup

      #新建空的yum源设置目录
      mkdir /etc/yum.repos.d

      #centos6系统
      wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS6-Base-163.repo
      wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo

      #centos7系统
      wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS7-Base-163.repo
      wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

      yum clean all
      yum makecache
      yum install epel-release openssh-clients
      yum install ansible ansible-doc -y

# 二、ansible运行
1、root用户运行

      ssh-keygen -t rsa
      ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.56.11
      ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.56.12
      ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.56.13

2、普通用户运行（私钥无密码）

      [ansible@linux-node2 ansible]$ useradd ansible -d /opt/ansible/
      [ansible@linux-node2 ansible]$ su - ansible
      
      [ansible@linux-node2 ansible]$ ssh-keygen -t rsa
      Generating public/private rsa key pair.
      Enter file in which to save the key (/opt/ansible//.ssh/id_rsa):
      Enter passphrase (empty for no passphrase):
      Enter same passphrase again:
      Your identification has been saved in /opt/ansible//.ssh/id_rsa.
      Your public key has been saved in /opt/ansible//.ssh/id_rsa.pub.
      The key fingerprint is:
      SHA256:mdZDJ4TPowiwPQtGZf0HoqAtY98rJN/fG+pfVBOCmGc ansible@linux-node2
      The key's randomart image is:
      +---[RSA 2048]----+
      |   .o.  o.o. .   |
      |  +.  oooE  . .  |
      | + = . oo+o .o   |
      |+.= =   .==o. .  |
      |.+...+ .Sooo     |
      | . o..... ..     |
      |  + . .  . .     |
      |   o o  o o      |
      |    . o+.+.      |
      +----[SHA256]-----+
      [ansible@linux-node2 ansible]$
      
      [ansible@linux-node2 ansible]$ ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.56.11
      [ansible@linux-node2 ansible]$ ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.56.12
      [ansible@linux-node2 ansible]$ ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.56.13
      
      
      #测试指定无密码的私钥登录
      [ansible@linux-node2 .ssh]$ ssh -i /opt/ansible/.ssh/id_rsa 'root@192.168.56.12'
      Last login: Sun Apr 28 20:48:54 2019 from 192.168.56.12
      [root@linux-node2 ~]#

3、普通用户运行（私钥有密码）

      [ansible@linux-node2 .ssh]$ ssh-keygen -t rsa
      Generating public/private rsa key pair.
      Enter file in which to save the key (/opt/ansible//.ssh/id_rsa):
      Enter passphrase (empty for no passphrase):  123456   ---这里输入私钥的证书
      Enter same passphrase again: 123456   ---这里确认输入私钥的证书
      Your identification has been saved in /opt/ansible//.ssh/id_rsa.
      Your public key has been saved in /opt/ansible//.ssh/id_rsa.pub.
      The key fingerprint is:
      SHA256:tJGz/QA36ASPeg0yMSLJ2FwdB7PChJQVg7GjwC7k+VQ ansible@linux-node2
      The key's randomart image is:
      +---[RSA 2048]----+
      |+=+BOo=o.        |
      |+o*= +.B o       |
      |.oo =E+ X o      |
      |=....= * O .     |
      |o+ .. . S o      |
      |. o  .     o     |
      |   .        .    |
      |                 |
      |                 |
      +----[SHA256]-----+
      [ansible@linux-node2 .ssh]$
      
      [ansible@linux-node2 ansible]$ ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.56.11
      [ansible@linux-node2 ansible]$ ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.56.12
      [ansible@linux-node2 ansible]$ ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.56.13
      
      
      #测试指定有密码的私钥登录
      [ansible@linux-node2 .ssh]$ ssh -i /opt/ansible/.ssh/id_rsa 'root@192.168.56.12'
      Enter passphrase for key '/opt/ansible/.ssh/id_rsa':  123456  --- 这里输入私钥密码
      Last login: Sun Apr 28 20:48:54 2019 from 192.168.56.12
      [root@linux-node2 ~]#

4、SSH私钥取消密码（passphrase ）

      1. 使用openssl命令去掉私钥的密码

      openssl rsa -in ~/.ssh/id_rsa -out ~/.ssh/id_rsa_new

      2. 备份旧私钥

      mv ~/.ssh/id_rsa ~/.ssh/id_rsa.backup

      3. 使用新私钥

      mv ~/.ssh/id_rsa_new ~/.ssh/id_rsa

      4. 设置权限

      chmod 600 ~/.ssh/id_rsa

5、配置ansible

a、编辑hosts文件

      cat > /etc/ansible/hosts <<EOF
      [mysql]
      node-02 ansible_ssh_host=192.168.56.12 ansible_ssh_user=root ansible_ssh_pass='123456' comment=zhangsan 

      [nginx]
      node-02 ansible_ssh_host=192.168.56.12 ansible_ssh_user=root ansible_ssh_pass='123456' comment=zhangsan 
      node-04 ansible_ssh_host=192.168.56.14 ansible_ssh_user=root ansible_ssh_pass='123456' comment=zhangsan
      EOF

b、编辑ansible.cfg

      cat > /etc/ansible/ansible.cfg <<EOF
      [defaults]
      inventory      = /etc/ansible/hosts
      library        = /usr/share/my_modules/
      module_utils   = /usr/share/my_module_utils/
      #计时模块在执行ansible-playbook时显示执行时长
      callback_whitelist = profile_tasks
      remote_tmp     = ~/.ansible/tmp
      local_tmp      = ~/.ansible/tmp
      roles_path    = /opt/ansible/playbooks/roles
      #关闭第一次使用ansible连接客户端是输入命令提示,关闭StrictHostKeyChecking检查
      host_key_checking = False
      deprecation_warnings=False
      timeout = 10
      log_path = /var/log/ansible.log
      #smart 表示默认收集 facts，但 facts 已有的情况下不会收集，即使用缓存 facts
      gathering = smart
      fact_caching = jsonfile
      fact_caching_connection=/tmp/.ansible_caching
      [inventory]
      [privilege_escalation]
      [paramiko_connection]
      [ssh_connection]
      #开启SSH长连接
      ssh_args = -C -o ControlMaster=auto -o ControlPersist=600s
      control_path_dir = ~/.ansible/cp
      control_path = %(directory)s/%%h-%%r
      #默认情况下，ansible的执行流程是把生成好的本地python脚本PUT到远程服务器然后运行。如果开启了pipelining，整个流程少了一个PUT脚本到远程服务器的步骤，直接在SSH的会话中进行，可以提高整个执行效率。
      pipelining = True
      #scp将代替用来为远程主机传输文件
      scp_if_ssh=False
      [persistent_connection]
      [accelerate]
      [selinux]
      [colors]
      [diff]
      EOF

      
6、测试运行

      [ansible@linux-node2 ansible]$ ansible node-02 -m command -a "date"
      node-02 | CHANGED | rc=0 >>
      Sun Apr 28 21:45:34 CST 2019
      
      或者
      
      [ansible@linux-node2 ~]$ ansible node-02 --private-key /opt/ansible/.ssh/id_rsa -m command -a "date"
      node-02 | CHANGED | rc=0 >>
      Sun Apr 28 21:34:17 CST 2019

# 三、配置参数验证

1、验证缓存配置
      
      使用setup模块获取主机信息
      [ansible@linux-node2 ~]$ ansible all -m setup
      
      #然后验证缓存jsonfile文件
      [ansible@linux-node2 .ansible_caching]$ pwd
      /tmp/.ansible_caching
      [ansible@linux-node2 .ansible_caching]$ ls
      node-02  node-04
      
2、验证长连接
      
      #执行一条指令
      [ansible@linux-node2 cp]$ ansible node-02 -m command -a "date"
      
      #验证目录是否有socket文件
      [ansible@linux-node2 cp]$ ls -l /opt/ansible/.ansible/cp/
      total 0
      srw-------. 1 ansible ansible 0 Apr 29 08:44 192.168.56.12-root
      srw-------. 1 ansible ansible 0 Apr 29 08:44 192.168.56.14-root
      
      列出当前主机可以使用的ansible模块：
      ansible-doc -l
      
      
# 四、添加时间计时模块

ansible中可以加入一个计时模块在执行ansible-playbook时显示执行时长。方便使用。

1、配置方法
```
cd /etc/ansible
mkdir callback_plugins
cd callback_plugins
wget https://raw.githubusercontent.com/jlafon/ansible-profile/master/callback_plugins/profile_tasks.py

注意：ansible2.0以上版本需在ansible.cfg中加入以下内容

[defaults] 下面加入
callback_whitelist = profile_tasks

再次执行ansbile-playbook时显示执行时长
```

2、测试结果
```
[root@linux-node2 node_exporter_install]# ansible-playbook node_exporter.yml  --tags="restart"

PLAY [node_exporter] **********************************************************************************************************************************************************

TASK [node_exporter_install : restart | restart node_exporter.service] ********************************************************************************************************
Monday 06 May 2019  06:44:35 +0800 (0:00:00.070)       0:00:00.070 ************
changed: [node-03]
changed: [node-02]

PLAY RECAP ********************************************************************************************************************************************************************
node-02                    : ok=1    changed=1    unreachable=0    failed=0
node-03                    : ok=1    changed=1    unreachable=0    failed=0

Monday 06 May 2019  06:44:36 +0800 (0:00:01.267)       0:00:01.338 ************
===============================================================================
node_exporter_install : restart | restart node_exporter.service -------------------------------------------------------------------------------------------------------- 1.27s
```




 参看文档： https://www.cnblogs.com/zhaojiankai/p/7655855.html

https://www.cnblogs.com/littlemonsters/p/5783672.html     SSH私钥取消密码（passphrase ）

https://blog.51cto.com/lxlxlx/1894386  ansible自动化部署之第三方模块添加（时间计时模块）

https://blog.csdn.net/JackLiu16/article/details/80577972  playbook debug 结合注册变量register打印日志

https://blog.csdn.net/qianggezhishen/article/details/53939188  ansible register 之用法

