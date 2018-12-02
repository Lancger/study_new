# 一、编写nginx的Dockerfile
```
# Base image
FROM docker.io/centos
# FROM centos:latest
# MAINTAINER
MAINTAINER 1151980610@qq.com
# running required command
RUN yum install -y nginx
EXPOSE 80
WORKDIR /
ENTRYPOINT [ "/usr/sbin/nginx", "-g", "daemon off;" ]
```

# 二、构建镜像
```
docker build -t nginx:base .
docker build -t nginx:base -f Dockerfile_base .
docker run -it -d --name=nginx -v /opt/Yearning/:/opt/Yearning/ python3:base /bin/bash
```
# 三、上传镜像到阿里云
```
#登陆命令
docker login --username=243533819@qq.com registry.cn-hangzhou.aliyuncs.com

#复制镜像ID并设置tag (或者tag repository:tag)
docker tag python3:base registry.cn-hangzhou.aliyuncs.com/lancger_ops/python3_base:v1.0.0

#上传镜像到阿里云镜像仓库
docker push registry.cn-hangzhou.aliyuncs.com/lancger_ops/python3_base:v1.0.0

#使用阿里云镜像
docker run -itd -v /opt/Yearning/:/opt/Yearning/ registry.cn-hangzhou.aliyuncs.com/lancger_ops/python3_base:v1.0.0
```
