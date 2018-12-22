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
systemctl restart docker

#设置docker服务开启自启
systemctl enable docker
#Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.

#查看是否成功设置docker服务开启自启
systemctl list-unit-files|grep docker
docker.service                                enabled

#关闭docker服务开启自启
systemctl disable docker
#Removed symlink /etc/systemd/system/multi-user.target.wants/docker.service.
```

## 2.脚本安装Docker
```bash
#2.1、Docker官方安装脚本
curl -sSL https://get.docker.com/ | sh

#这个脚本会添加docker.repo仓库并且安装Docker

#2.2、阿里云的安装脚本
curl -sSL http://acs-public-mirror.oss-cn-hangzhou.aliyuncs.com/docker-engine/internet | sh -

#2.3、DaoCloud 的安装脚本
curl -sSL https://get.daocloud.io/docker | sh

```

## 3.配置docker
```bash
#3.1、修改docker配置文件
vim /usr/lib/systemd/system/docker.service

#1、在ExecStart前面添加下面这一行
EnvironmentFile=/etc/sysconfig/docker
ExecStart=/usr/bin/dockerd

#2、修改ExecStart=/usr/bin/dockerd 为 
ExecStart=/usr/bin/dockerd $OPTIONS
```
### 3.1 最终的配置

    注意，有变量的地方需要使用转义符号
    
    cat > /usr/lib/systemd/system/docker.service << EOF
    [Unit]
    Description=Docker Application Container Engine
    Documentation=https://docs.docker.com
    After=network-online.target firewalld.service
    Wants=network-online.target

    [Service]
    Type=notify
    # the default is not to use systemd for cgroups because the delegate issues still
    # exists and systemd currently does not support the cgroup feature set required
    # for containers run by docker
    EnvironmentFile=/etc/sysconfig/docker
    ExecStart=/usr/bin/dockerd \$OPTIONS
    ExecReload=/bin/kill -s HUP \$MAINPID
    # Having non-zero Limit*s causes performance problems due to accounting overhead
    # in the kernel. We recommend using cgroups to do container-local accounting.
    LimitNOFILE=infinity
    LimitNPROC=infinity
    LimitCORE=infinity
    # Uncomment TasksMax if your systemd version supports it.
    # Only systemd 226 and above support this version.
    #TasksMax=infinity
    TimeoutStartSec=0
    # set delegate yes so that systemd does not reset the cgroups of docker containers
    Delegate=yes
    # kill only the docker process, not all processes in the cgroup
    KillMode=process
    # restart the docker process if it exits prematurely
    Restart=on-failure
    StartLimitBurst=3
    StartLimitInterval=60s

    [Install]
    WantedBy=multi-user.target
    EOF

### 3.2、配置镜像加速器
    cat > /etc/sysconfig/docker << EOF
    OPTIONS='--selinux-enabled --registry-mirror=https://i37dz0y4.mirror.aliyuncs.com'
    EOF

### 3.3、重新加载docker的配置文件
    systemctl daemon-reload
    
## 4.通过测试镜像运行一个容器来验证Docker是否安装正确
```bash
docker run hello-world
```
