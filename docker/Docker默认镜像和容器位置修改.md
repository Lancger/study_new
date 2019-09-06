# 一、软链接

默认情况下Docker的存放位置为： /var/lib/docker
可以通过下面命令查看具体位置：

```
sudo docker info | grep "Docker Root Dir"

# 1、停掉docker服务
systemctl stop docker
service docker stop

# 2、然后移动整个/var/lib/docker目录到目的路径：
mv /var/lib/docker /data0/docker
ln -s /data0/docker /var/lib/docker

这时候启动Docker时发现存储目录依旧是/var/lib/docker，但是实际上是存储在数据盘的，你可以在数据盘上看到容量变化。

```

# 二、修改镜像和容器的存放路径

指定镜像和容器存放路径的参数是 –graph=/var/lib/docker ，我们只需要修改配置文件指定启动参数即可。

Docker 的配置文件可以设置大部分的后台进程参数，在各个操作系统中的存放位置不一致，在 Ubuntu 中的位置是： /etc/default/docker，在 CentOS 中的位置是： /etc/sysconfig/docker。

```
# 1、如果是 CentOS 则添加下面这行：
OPTIONS=--graph="/data/docker" --selinux-enabled -H fd://

# 2、如果是 Ubuntu 则添加下面这行（因为 Ubuntu 默认没开启 selinux）：
OPTIONS=--graph="/data/docker" -H fd://
# 或者
DOCKER_OPTS="-g /data/docker"
```

参考资料：

https://blog.csdn.net/u014527619/article/details/81079637
