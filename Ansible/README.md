# 一、Ansible介绍及安装部署

      wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS7-Base-163.repo
      yum clean all
      yum makecache
      yum install epel-release
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
      remote_tmp     = ~/.ansible/tmp
      local_tmp      = ~/.ansible/tmp
      roles_path    = /opt/ansible/roles
      #关闭第一次使用ansible连接客户端是输入命令提示
      host_key_checking = False //关闭StrictHostKeyChecking检查
      timeout = 10
      log_path = /var/log/ansible.log
      gathering = smart  //smart 表示默认收集 facts，但 facts 已有的情况下不会收集，即使用缓存 facts
      fact_caching = jsonfile
      fact_caching_connection=/tmp/.ansible_caching
      [inventory]
      [privilege_escalation]
      [paramiko_connection]
      [ssh_connection]
      #开启SSH长连接
      ssh_args = -C -o ControlMaster=auto -o ControlPersist=600s // ControlPersist 即持久化 socket，一次验证，长连接时间保持10分钟
      control_path_dir = ~/.ansible/cp
      control_path = %(directory)s/%%h-%%r
      #默认情况下，ansible的执行流程是把生成好的本地python脚本PUT到远程服务器然后运行。如果开启了pipelining，整个流程少了一个PUT脚本到远程服务器的步骤，直接在SSH的会话中进行，可以提高整个执行效率。
      pipelining = True //加速 Ansible 执行速度
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


      4.使用模块
      ansible 192.168.56.11 -m command -a "date"
      
      列出当前主机可以使用的ansible模块：
      ansible-doc -l
      
# 三、roles示例

      假设有3台主机，192.168.56.11主机上安装MySQL，192.168.56.12上安装nginx，192.168.56.13上安装MySQL和nginx。我们建立两个角色websrvs和dbsrvs，然后应用到这几个主机上。
      
      1. 创建roles的必需目录 

      mkdir -pv /opt/ansible_playbooks/roles/{websrvs,dbsrvs}/{tasks,files,templates,meta,handlers,vars}
      
      [root@linux-node1 opt]# tree  ansible_playbooks/
      ansible_playbooks/
      └── roles
          ├── dbsrvs
          │   ├── files
          │   ├── handlers
          │   ├── meta
          │   ├── tasks
          │   ├── templates
          │   └── vars
          └── websrvs
              ├── files
              ├── handlers
              ├── meta
              ├── tasks
              ├── templates
              └── vars

      15 directories, 0 files
      #每个role下面有个目录叫meta，在里面可以新建文件main.yml，在文件中可以设置该role和其它role之前的关联关系。
      
      2. 配置角色
      
     （1）配置角色websrvs
      [root@node1 opt]# cd ansible_playbooks/roles/
      [root@node1 roles]# cd websrvs/
      [root@node1 websrvs]# ls
      files  handlers  meta  tasks  templates  vars
      
      a. 将nginx.conf配置文件上传到files目录下，我这里假设nginx.conf每台主机都是一样的，实际上应该用模板，先用一样的配置文件举例
      
      [root@node1 websrvs]# cp /etc/nginx.conf files/
      
      直接复制的静态文件都放在files目录下。打算用模板文件的都放在templates目录下。
      
      b.编写任务列表tasks
      [root@node1 websrvs]# vim tasks/main.yml
      - name: install nginx package
        yum: name=nginx
      - name: install configuration file
        copy: src=nginx.conf dest=/etc
        tags:
        - conf
        notify:
        - restart nginx
      - name: start nginx
        service: name=nginx state=started
        
      c.由于上面的tasks中定义了notify，所以要定义handlers
      [root@node1 websrvs]# vim handlers/main.yml
      - name: restart nginx
        service: name=nginx state=restarted
          
      如果需要定义变量，则在vars目录下创建main.yml文件，在文件中写入变量，以key:value的形式定义，比如：

      http_port: 8080

    （2）配置角色dbsrvs

      [root@node1 roles]# cd dbsrvs/
      [root@node1 dbsrvs]# ls
      files  handlers  meta  tasks  templates  vars
      
      a.将MySQL配置文件上传到files目录下。
      
      b.编写任务列表tasks
      [root@node1 dbsrvs]# vim tasks/main.yml
      - name: install mysql-server package
        yum: name=mysql-server state=latest
      - name: install configuration file
        copy: src=my.cnf dest/etc/my.cnf
        tags:
        - conf
        notify:
        - restart mysqld
      - name:
        service: name=mysqld enabled=true state=started
        
      c.定义handlers
      [root@node1 dbsrvs]# vim handlers/main.yml
      - name: restart mysqld
        service: name=mysqld state=restarted
        
     （3）定义playbook
     [root@node1 ansible_playbooks]# vim web.yml
      - hosts: 192.168.56.11
        roles:
        - websrvs 

      [root@node1 ansible_playbooks]# vim db.yml
      - hosts: 192.168.56.12
        roles:
        - dbsrvs 

      [root@node1 ansible_playbooks]# vim site.yml
      - hosts: 192.168.56.13
        roles:
        - websrvs
        - dbsrvs
        
        
        运行：

      [root@node1 ansible_playbooks]# ansible-playbook web.yml
      [root@node1 ansible_playbooks]# ansible-playbook db.yml
      [root@node1 ansible_playbooks]# ansible-playbook site.yml

      当然也可以把这些内容写入同一个playbook中。playbook的名字可以自定义。




      
 参看文档： https://www.cnblogs.com/zhaojiankai/p/7655855.html

https://www.cnblogs.com/littlemonsters/p/5783672.html     SSH私钥取消密码（passphrase ）

