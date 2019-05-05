# 一、file模块
```
设定文件属性
force：需要在两种情况下强制创建软链接，一种是源文件不存在，但之后会建立的情况下；另一种是目标软链接已存在，需要先取消之前的软链，然后创建新的软链，有两个选项：yes|no
group：定义文件/目录的属组
mode：定义文件/目录的权限
owner：定义文件/目录的属主
path：必选项，定义文件/目录的路径
recurse：递归设置文件的属性，只对目录有效
src：被链接的源文件路径，只应用于state=link的情况
dest：被链接到的路径，只应用于state=link的情况
state：
      directory：如果目录不存在，就创建目录
      file：即使文件不存在，也不会被创建
      link：创建软链接
      hard：创建硬链接
      touch：如果文件不存在，则会创建一个新的文件，如果文件或目录已存在，则更新其最后修改时间
      absent：删除目录、文件或者取消链接文件
      
## 远程文件符号链接创建
[root@Ansible ~]# ansible all -m file -a "src=/etc/resolv.conf dest=/tmp/resolv.conf state=link"
192.168.102.133 | SUCCESS => {
    "changed": true, 
    "dest": "/tmp/resolv.conf", 
    "gid": 0, 
    "group": "root", 
    "mode": "0777", 
    "owner": "root", 
    "size": 16, 
    "src": "/etc/resolv.conf", 
    "state": "link", 
    "uid": 0
}
## 远程文件信息查看
[root@Ansible ~]# ansible all -m command -a "ls -al /tmp/resolv.conf"
192.168.102.133 | SUCCESS | rc=0 >>
lrwxrwxrwx 1 root root 16 4月  10 23:56 /tmp/resolv.conf -> /etc/resolv.conf
## 远程文件符号链接删除
[root@Ansible ~]# ansible all -m file -a "path=/tmp/resolv.conf state=absent"
192.168.102.133 | SUCCESS => {
    "changed": true, 
    "path": "/tmp/resolv.conf", 
    "state": "absent"
}

```

# 二、yum模块
```
用于管理节点安装软件所使用

ansible-doc -s yum action: yum
      state=       指定是安装(`present', `latest'), 还是卸载 remove (`absent')
      disablerepo= 如果有多个yum源可能禁用一个
      name=        要安装的包名(如果只写包名，就安装最新的包，如果想指定安装什么片本的就包名+版本号）
      enablerepo=  要启用的repo
      list=        列表(跟playbook相关）
      disable_gpg_check=  是否开启gpg_check
      conf_file=    可以用其它服务器上的配置文件，而不使用本地的配置文件

例：所有的主机都安装上corosync.

[root@Ansible ~]# ansible all -m yum -a "state=present name=corosync"
[root@Ansible ~]# ansible all -m yum -a 'name=ntp state=present'

卸载的软件只需要将 name=ntp state=absent

安装特定版本 name=nginx-1.6.2 state=present

指定某个源仓库安装软件包name=htop enablerepo=epel state=present

更新软件到最新版 name=nginx state=latest
```

# 三、service 模块
```
作用：管理服务

参数
       name=       服务名称
       state=        状态
           started       启动
           stopped      停止
           restarted    重启
           enabled=    [yes|no] 是否随系统启动
           runlevel=             运行级别

示例：

[root@Ansible ~]# ansible all -m service -a"name=httpd state=started enabled=yes runlevel=5"
192.168.102.133 | SUCCESS => {
    "changed": true, 
    "enabled": true, 
    "name": "httpd", 
    "state": "started"
}

记得针对Centos7就不要使用这个模块了。
```

# 四、实站案例
```
执行shell
获取web组里得eth0接口信息
ansible web -a "ifconfig eth0"
执行ifconfig eth0 命令，ansible模块 默认是command，它不会通过shell进行处理，
所以像$ HOME和像“<”，“>”，“|”，“;” 和“＆”将不工作（如果您需要这些功能，请使用shell模块）。
以shell解释器执行脚本
ansible web -m shell -a "ifconfig eth0|grep addr"
以raw模块执行脚本
ansible web -m raw -a "ifconfig eth0|grep addr
将本地脚本传送到远程节点上运行
ansible web -m script -a ip.sh
传输文件
拷贝本地的/etc/hosts 文件到web组所有主机的/tmp/hosts（空目录除外）
ansible web -m copy -a "src=/etc/hosts dest=/tmp/hosts"
拷贝本地的ntp文件到目的地址，设置其用户及权限，如果目标地址存在相同的文件，则备份源文件。
ansible web -m copy -a "src=/mine/ntp.conf dest=/etc/ntp.conf owner=root group=root mode=644 backup=yes force=yes"
file 模块允许更改文件的用户及权限
ansible web -m file -a "dest=/tmp/a.txt mode=600 owner=user group=user"
使用file 模块创建目录，类似mkdir -p
ansible web -m file -a "dest=/tmp/test mode=755 owner=user group=user state=directory"
使用file 模块删除文件或者目录
ansible web -m file -a "dest=/tmp/test state=absent"
创建软连接，并设置所属用户和用户组
ansible web -m file -a  "src=/file/to/link/to dest=/path/to/symlink owner=user group=user state=link"
touch 一个文件并添加用户读写权限，用户组去除写执行权限，其他组减去读写执行权限
ansible web -m file -a  "path=/etc/foo.conf state=touch mode='u+rw,g-wx,o-rwx'"
管理软件包
apt、yum 模块分表用于管理Ubuntu 系列和RedHat 系列系统软件包
更新仓库缓存，并安装"foo"
ansible web -m apt -a "name=foo update_cache=yes"
删除 "foo"
ansible web -m apt -a "name=foo state=absent"
安装  "foo"
ansible web -m apt -a "name=foo state=present"
安装  1.0版本的 "foo"
ansible web -m apt -a "name=foo=1.00 state=present"
安装最新得"foo"
ansible web -m apt -a "name=foo state=latest"
注释：Ansible 支持很多操作系统的软件包管理，使用时-m 指定相应的软件包管理工具模块，如果没有这样的模块，可以自己定义类似的模块或者使用command 模块来安装软件包
安装 最新的 Apache
ansible web -m yum -a  "name=httpd state=latest"
删除apache
ansible web -m yum -a  "name=httpd state=absent"
从testing 仓库中安装最后一个版本得apache
ansible web -m yum -a  "name=httpd enablerepo=testing state=present"
更新所有的包
ansible web -m yum -a  "name=* state=latest"
安装远程的rpm包
ansible web -m yum -a  "name=http://nginx.org/packages/centos/6/noarch/RPMS/nginx-release-centos-6-0.el6.ngx.noarch.rpm state=present"
安装  'Development tools' 包组
ansible web -m yum -a  "name='@Development tools' state=present"
用户和用户组
添加用户 'user'并设置其 uid 和主要的组'admin'
ansible web -m user -a "name=user  comment='I am user ' uid=1040 group=admin"
添加用户 'user'并设置其登陆shell，并将其假如admins和developers组
ansible web -m user -a "name=user shell=/bin/bash groups=admins,developers append=yes"
删除用户 'user '
ansible web -m user -a "name=user state=absent remove=yes"
创建 user用户得   2048-bit SSH key，并存放在 ~user/.ssh/id_rsa
ansible web -m user -a "name=user generate_ssh_key=yes ssh_key_bits=2048 ssh_key_file=.ssh/id_rsa"
设置用户过期日期
ansible web -m user -a "name=user shell=/bin/zsh groups=nobdy expires=1422403387"
创建test组，并设置git为1000
ansible web -m group -a "name=test gid=1000 state=present"
删除test组
ansible web -m group -a "name=test state=absent"
源码部署
Ansible 模块能够通知变更，当代码更新时，可以告诉Ansible 做一些特定的任务，比如从git 部署代码然后重启apache 服务等
ansible web-m git -a "repo=https://github.com/Icinga/icinga2.git dest=/tmp/myapp   version=HEAD"
服务管理
确保web组所有主机的httpd 是启动的
ansible web-m service -a "name=httpd state=started"
重启web组所有主机的httpd 服务
ansible web-m service -a "name=httpd state=restarted"
确保web组所有主机的httpd 是关闭的
ansible web-m service -a "name=httpd state=stopped"
后台运行
长时间运行的操作可以放到后台执行，ansible 会检查任务的状态；在主机上执行的同一个任
务会分配同一个job ID
后台执行命令3600s，-B 表示后台执行的时间
ansible all -B 3600 -a "/usr/bin/long_running_operation --do-stuff"
检查任务的状态
ansible all -m async_status -a "jid=123456789"
后台执行命令最大时间是1800s 即30 分钟，-P 每60s 检查下状态默认15s
ansible all -B 1800 -P 60 -a "/usr/bin/long_running_operation --do-stuff"
定时任务
每天5点，2点得时候执行 ls -alh > /dev/null
ansible test -m cron -a "name='check dirs' minute='0' hour='5,2' job='ls -alh > /dev/null'"
搜集系统信息
搜集主机的所有系统信息
ansible all -m setup
搜集系统信息并以主机名为文件名分别保存在/tmp/facts 目录
ansible all -m setup --tree /tmp/facts
搜集和内存相关的信息
ansible all -m setup -a 'filter=ansible_*_mb'
搜集网卡信息
ansible all -m setup -a 'filter=ansible_eth[0-2]'
```

参考文档：

https://my.oschina.net/u/3413282/blog/876231
