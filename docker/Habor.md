# 一、Habor部署配置
```
export version="v1.8.2"

wget https://storage.googleapis.com/harbor-releases/release-1.8.0/harbor-offline-installer-v1.8.2.tgz

tar xf harbor-offline-installer-${version}.tgz
cd harbor/

vim harbor.cfg
hostname = hub.wow
其他默认(http协议)

./install.sh
安装成功后，可以通过http://hub.wow/访问
```

# 二、Docker客户端使用
```
由于Harbor默认使用的http协议,故需要在Docker client上的Dockerd服务增加–insecure-registry hub.wow
Centos7修改方式为:

vim /lib/systemd/system/docker.service
ExecStart=/usr/bin/dockerd  --insecure-registry hub.wow

systemctl daemon-reload
systemctl reload docker

[root@localhost harbor]# docker login -u admin -p Harbor12345 hub.wow
官方仓库下载busybox镜像
[root@localhost harbor]# docker pull busybox 
[root@localhost harbor]# docker images
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
busybox                     latest              efe10ee6727f        2 weeks ago         1.13MB
本地基于busybox:latest创建标记hub.wow/busybox:latest
[root@localhost harbor]# docker tag busybox:latest hub.wow/library/busybox:latest

推送本地镜像busybox:latest 到hub.wow私有仓库
[root@localhost harbor]# docker push hub.wow/library/busybox:latest

```

# 三、Harbor服务管理
```
cd harbor/
docker-compose -f ./docker-compose.yml [ up|down|ps|stop|start ]
```
