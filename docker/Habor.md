# 基础准备
```
chattr -i /etc/passwd* && chattr -i /etc/group* && chattr -i /etc/shadow* && chattr -i /etc/gshadow*
chattr +i /etc/passwd* && chattr +i /etc/group* && chattr +i /etc/shadow* && chattr +i /etc/gshadow*
```
# 一、Habor部署配置
```
cd /usr/local/src
export version="v1.8.2"
wget https://storage.googleapis.com/harbor-releases/release-1.8.0/harbor-offline-installer-v1.8.2.tgz
tar xf harbor-offline-installer-${version}.tgz
cd harbor/

vim harbor.yml
hostname = reg.hub.com

其他默认(http协议)

./install.sh
安装成功后，可以通过http://reg.hub.com访问
```

# 二、Docker客户端使用
```
由于Harbor默认使用的http协议,故需要在Docker client上的Dockerd服务增加–insecure-registry hub.wow

# 方式一：
Centos7修改方式为:
vim /lib/systemd/system/docker.service

ExecStart=/usr/bin/dockerd  --insecure-registry reg.hub.com

systemctl daemon-reload
systemctl reload docker

# 方式二：
cat > /etc/docker/daemon.json << \EOF
{
  "registry-mirrors": [
    "https://dockerhub.azk8s.cn",
    "https://reg-mirror.qiniu.com"
  ],
  "insecure-registries": ["reg.hub.com"]
}
EOF

systemctl daemon-reload
systemctl reload docker

# 测试
[root@localhost harbor]# docker login -u admin -p Harbor12345 reg.hub.com

官方仓库下载busybox镜像
[root@localhost harbor]# docker pull busybox 
[root@localhost harbor]# docker images
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
busybox                     latest              efe10ee6727f        2 weeks ago         1.13MB

本地基于busybox:latest创建标记reg.hub.com/busybox:latest
[root@localhost harbor]# docker tag busybox:latest reg.hub.com/library/busybox:latest

推送本地镜像busybox:latest 到hub.wow私有仓库
[root@localhost harbor]# docker push reg.hub.com/library/busybox:latest

```

# 三、Harbor服务管理
```
cd harbor/
docker-compose -f ./docker-compose.yml [ up|down|ps|stop|start ]
```

参考文档：

https://www.centos.bz/2017/08/centos-docker-registry-harbor-install/
