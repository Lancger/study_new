# 一、编写dockerfile
```
FROM registry.cn-hangzhou.aliyuncs.com/lancger_ops/python3_base:v1.0.0
WORKDIR /opt/
RUN yum -y install nginx
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
