```
FROM docker.io/centos
# FROM centos:latest
# yearning
WORKDIR /opt/
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
    # 下载源码
    # cd opt \
    # && git clone https://github.com/cookieY/Yearning.git \
    # 安装nginx
    && yum -y install nginx
# 拷贝编译好的前端文件
ADD dist/* /var/lib/nginx/html/
# 拷贝一份deploy.conf
ADD Yearning/src/deploy.conf.template /opt/Yearning/src/deploy.conf
# 安装项目所需的requirements.txt
ADD Yearning/src/requirements.txt /opt/Yearning/src/requirements.txt
# 安装requirements.txt
RUN pip3 install -r /opt/Yearning/src/requirements.txt -i https://mirrors.ustc.edu.cn/pypi/web/simple/
# 增加yearning的nginx配置文件
ADD yearning.conf /etc/nginx/conf.d/
# 增加启动脚本
ADD start_yearning.sh /opt/Yearning
# 挂载逻辑卷
VOLUME /opt/Yearning/ /opt/Yearning/
# port
EXPOSE 80
EXPOSE 8000
# start service
ENTRYPOINT bash /opt/Yearning/start_yearning.sh && bash
```
