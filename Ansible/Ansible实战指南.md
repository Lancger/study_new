# file模块
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

参考文档：

https://my.oschina.net/u/3413282/blog/876231
