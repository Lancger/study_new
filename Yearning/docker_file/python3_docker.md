# 一、编写python3的Dockerfile
```
# Base image
FROM docker.io/centos
# FROM centos:latest
# MAINTAINER
MAINTAINER 1151980610@qq.com
# running required command
RUN yum install -y wget \
    # 变更163yum源
    && wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS7-Base-163.repo \
    && yum install -y wget readline readline-devel gcc gcc-c++ zlib zlib-devel openssl openssl-devel sqlite-devel python-devel \
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
CMD ["python3"]
```

# 二、构建镜像
```
docker build -t python3:base .
docker build -t python3:base -f Dockerfile_base .
docker run -it -d --name=python3 -v /opt/Yearning/:/opt/Yearning/ yearning:bash /bin/bash
```
