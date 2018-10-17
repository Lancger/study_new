# study_docker

## 1.安装Docker

第一步：使用国内Docker源
```
cd /etc/yum.repos.d/
wget https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
 ```

第二步：Docker安装：
```
yum install -y docker-ce
```

第三步：启动后台进程：
```
#启动docker服务
systemctl start docker

#设置docker服务开启自启
systemctl enable docker
#Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.

#关闭docker服务开启自启
systemctl disable docker
#Removed symlink /etc/systemd/system/multi-user.target.wants/docker.service.
```

## 2.脚本安装Docker
```bash
Docker官方安装脚本
$ curl -sSL https://get.docker.com/ | sh
#这个脚本会添加docker.repo仓库并且安装Docker

2.2、阿里云的安装脚本
curl -sSL http://acs-public-mirror.oss-cn-hangzhou.aliyuncs.com/docker-engine/internet | sh -

2.3、DaoCloud 的安装脚本
curl -sSL https://get.daocloud.io/docker | sh

```

## 3.配置docker
```bash
3.1、修改docker配置文件
$ vim /usr/lib/systemd/system/docker.service

1、在ExecStart前面添加下面这一行
EnvironmentFile=/etc/sysconfig/docker
ExecStart=/usr/bin/dockerd

2、修改ExecStart=/usr/bin/dockerd 为 
ExecStart=/usr/bin/dockerd $OPTIONS

3.2、重新加载docker的配置文件
$ systemctl daemon-reload

3.3、配置镜像加速器
$ vim /etc/sysconfig/docker
添加下面这一行
OPTIONS='--selinux-enabled --registry-mirror=http://7cc19f93.m.daocloud.io'
