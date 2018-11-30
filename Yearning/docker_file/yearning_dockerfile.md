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

# 二、构建镜像
```
docker build -t yearning:base -f Dockerfile_yearning .
```
# 三、上传镜像
```
#登陆命令
docker login --username=243533819@qq.com registry.cn-hangzhou.aliyuncs.com

#复制镜像ID并设置tag (或者tag repository:tag)
docker tag yearning:base registry.cn-hangzhou.aliyuncs.com/lancger_ops/yearning_base:v1.0.0

#上传镜像到阿里云镜像仓库
docker push registry.cn-hangzhou.aliyuncs.com/lancger_ops/yearning_base:v1.0.0

#使用阿里云镜像
docker run -itd -v /opt/Yearning/:/opt/Yearning/ registry.cn-hangzhou.aliyuncs.com/lancger_ops/yearning_base:v1.0.0

docker run -itd registry.cn-hangzhou.aliyuncs.com/lancger_ops/yearning_base:v1.0.0

```
