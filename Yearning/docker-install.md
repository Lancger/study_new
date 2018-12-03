# 一、Docker常用操作
```
#下载镜像
docker pull centos

#创建名为test的centos容器
docker run -it -d --name=yearning centos /bin/bash

docker run -d --name=yearning -v /data/Yearning/:/mnt/Yearning -p 8000:8000 -p 2222:22 yearning:base

#进入容器
docker exec -it yearning /bin/bash

#想要删除untagged images，也就是那些id为<None>的image的话可以用
docker rmi $(docker images | grep none | awk '{print $3}' | sort -r)
docker rmi $(docker images | grep "^<none>" | awk "{print $3}")

#要删除全部image的话
docker rmi $(docker images -q)
docker rmi -f $(docker images -q)

#构建容器镜像
docker build -t yearning:base .

#删除容器
docker rm -f `docker ps -a -q`
```

# 二、镜像构建

```
#服务启动脚本
start_yearning.sh

/usr/sbin/nginx
cd /opt/Yearning/src
sed -i "s/ipaddress =.*/ipaddress=$HOST/" deploy.conf
sed -i "s/address =.*/address=$MYSQL_ADDR/" deploy.conf
sed -i "s/username =.*/username=$MYSQL_USER/" deploy.conf
sed -i "s/password =.*/password=$MYSQL_PASSWORD/" deploy.conf
/usr/local/python3.6/bin/gunicorn settingConf.wsgi:application -b 0.0.0.0:8000 -w 2


#本地镜像
FROM yearning_local
MAINTAINER cookieYe 2018-9-17
EXPOSE 8000
EXPOSE 80
ENTRYPOINT start_yearning.sh

```

# 三、Dockerfile编写
```
FROM docker.io/centos
# FROM centos:latest
# yearning
# WORKDIR /opt/
RUN yum install -y wget \
    # 变更163yum源
    && wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS7-Base-163.repo \
    && yum clean all \
    && yum makecache \
    && yum install -y wget git readline readline-devel gcc gcc-c++ zlib zlib-devel openssl openssl-devel sqlite-devel python-devel \
    && yum -y install epel-release \
    # python3.6.6
    && cd /usr/local/src \
    && wget https://www.python.org/ftp/python/3.6.6/Python-3.6.6.tgz \
    && tar -xzf Python-3.6.6.tgz \
    && cd Python-3.6.6 \
    && ./configure --prefix=/usr/local/python3.6 --enable-shared \
    && make && make install \
    && ln -s /usr/local/python3.6/bin/python3.6 /usr/bin/python3 \
    && ln -s /usr/local/python3.6/bin/pip3 /usr/bin/pip3 \
    && ln -s /usr/local/python3.6/bin/pyvenv /usr/bin/pyvenv \
    && cp /usr/local/python3.6/lib/libpython3.6m.so.1.0 /usr/local/lib \
    && cd /usr/local/lib \
    && ln -s libpython3.6m.so.1.0 libpython3.6m.so \
    && echo '/usr/local/lib' >> /etc/ld.so.conf \
    && /sbin/ldconfig \
    # pip升级
    && pip3 install --upgrade pip \
    # 下载源码
    && cd /opt \
    && git clone https://github.com/cookieY/Yearning.git \
    # 安装nginx
    && yum -y install nginx
# 拷贝编译好的前端文件
ADD dist/ /var/lib/nginx/
# 增加yearning的nginx配置文件
ADD nginx.conf /etc/nginx/nginx.conf
# 拷贝一份deploy.conf
ADD Yearning/src/deploy.conf.template /opt/Yearning/src/deploy.conf
# 安装依赖
RUN pip3 install -r /opt/Yearning/src/requirements.txt -i https://mirrors.ustc.edu.cn/pypi/web/simple/
# 增加启动脚本
ADD start_yearning.sh /opt/
# 挂载逻辑卷
#VOLUME /opt/Yearning/ /tmp/
# port
EXPOSE 80
EXPOSE 8000
# start service
ENTRYPOINT bash /opt/start_yearning.sh && bash
```

## 四、镜像构建
```
docker build -t yearning:base .

docker build -t yearning:local -f Dockerfile .

docker build -t yearning:base -f Dockerfile .
```

## 五、上传镜像到阿里云
```
#登陆命令
docker login --username=243533819@qq.com registry.cn-hangzhou.aliyuncs.com

#复制镜像ID并设置tag (或者tag repository:tag)
docker tag yearning:base registry.cn-hangzhou.aliyuncs.com/lancger_ops/yearning_base:v1.0.0

#上传镜像到阿里云镜像仓库
docker push registry.cn-hangzhou.aliyuncs.com/lancger_ops/yearning_base:v1.0.0

#使用阿里云镜像
docker run -itd registry.cn-hangzhou.aliyuncs.com/lancger_ops/yearning_base:v1.0.0

```
