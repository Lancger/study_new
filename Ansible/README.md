# Ansible介绍及安装部署

  wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS7-Base-163.repo
  yum clean all
  yum makecache
  yum install epel-release
  yum install ansible -y
  
  ssh-keygen -t rsa
  ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.56.11
  ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.56.12
  ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.56.13
  
  
  #vim /etc/ansible/hosts
  [mysql]
  192.168.56.12
  
  [nginx]
  192.168.56.11
  192.168.56.12
  192.168.56.13
  
  
