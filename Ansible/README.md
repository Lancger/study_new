# Ansible介绍及安装部署

      wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS7-Base-163.repo
      yum clean all
      yum makecache
      yum install epel-release
      yum install ansible ansible-doc -y

# ansible运行
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
      

4、配置ansible

      #vim /etc/ansible/hosts
      [mysql]
      192.168.56.12

      [nginx]
      192.168.56.11
      192.168.56.12
      192.168.56.13
      
      4.使用模块
      ansible 192.168.56.11 -m command -a "date"
      
      
      列出当前主机可以使用的ansible模块：
      ansible-doc -l
      
# 二、roles示例

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


