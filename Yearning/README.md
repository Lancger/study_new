# 一、部署文档
```bash
#1.先安装 docker-compose
curl -L "https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
docker-compose --version

#2.克隆源码
cd /opt/
git clone https://github.com/cookieY/Yearning.git

#下载Yearning,进入install/yearning-docker-compose目录
cd /opt/Yearning/install/yearning-docker-compose

docker-compose up -d

#要删除全部image的话
docker rmi $(docker images -q)

docker exec -it [容器ID]  /bin/bash   建议使用

# 删除所有容器 
docker rm -f `docker ps -a -q` 

# 查看容器信息
docker inspect ID
```

    请注意本地不要占用8080和8000端口 如需要更改端口可再docker-compose.yml文件中更改，3306和8000端口不可更改！docker-compose并不能确定容器的依赖关系，所以如果执行后无法登陆，请使用docker-compose restart yearning重启容器

    之后访问8080端口。

    默认用户admin 密码: Yearning_admin

    登陆后请通过设置页面设置inception及其他配置信息

# 二、安装percona toolkit工具（pt-online-schema-change 在线执行DDL操作,不会阻塞读写操作）

```
#安装percona toolkit

wget https://www.percona.com/redir/downloads/percona-release/redhat/percona-release-0.1-3.noarch.rpm
rpm -Uvh percona-release-0.1-3.noarch.rpm
yum install percona-toolkit
```

# 三、inception

因为db和nginx+python放在2个容器里面，用127.0.0.1是访问不道到（nginx+python的容器没有mysql），所以inception放在数据库所在服务器
```bash
tar xvf inception.tar -C /usr/local/

nohup /usr/local/inception/bin/Inception --defaults-file=/usr/local/inception/bin/inc.cnf &

mysql -uroot -h192.168.56.138 -P6669

mysql> inception get variables;

nohup /usr/local/inception/bin/Inception --defaults-file=/usr/local/inception/bin/inc.cnf &

GRANT ALL PRIVILEGES ON *.* TO root@"192.168.56.138" IDENTIFIED BY "123456";

docker run -d -e HOST=192.168.56.138 -e MYSQL_ADDR=192.168.56.10 -e MYSQL_USER=root -e MYSQL_PASSWORD=123456 -p8080:80 -p8000:8000 registry.cn-hangzhou.aliyuncs.com/cookie/yearning:v1.3.3
```

# 三、mysql容器时区问题

```
解决办法一：

TZ: Asia/Shanghai

my.cnf配置添加如下配置
log_timestamps = SYSTEM

解决办法二：

- /etc/localtime:/etc/localtime:ro
- /etc/timezone:/etc/timezone:ro

my.cnf配置添加如下配置
log_timestamps = SYSTEM

最终完整例子：
version: '2'

services:
  db:
    image: mysql:5.7
    volumes:
      - ./docker/etc/mysql/:/etc/mysql/conf.d/
      - ./db_data/:/var/lib/mysql/
      - ./init-sql/:/docker-entrypoint-initdb.d/
      - /etc/localtime:/etc/localtime:ro
      - /etc/timezone:/etc/timezone:ro
    restart: always
    ports:
      - "3306:3306"
    environment:
      TZ: Asia/Shanghai
      MYSQL_ROOT_PASSWORD: 123456
      MYSQL_DATABASE: Yearning
      MYSQL_USER: yearning
      MYSQL_PASSWORD: yearning
  yearning:
    image: registry.cn-hangzhou.aliyuncs.com/cookie/yearning:v1.3.4
    depends_on:
      - db
    ports:
      - "8080:80"
      - "8000:8000"
    environment:
      HOST: localhost
      MYSQL_PASSWORD: 123456
      MYSQL_USER: root
      MYSQL_ADDR: db

```


参考文档：

http://blog.itpub.net/27000195/viewspace-2120332/     Inception相关功能学习 

