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


# 二、inception

因为db和nginx+python放在2个容器里面，用127.0.0.1是访问不道到（nginx+python的容器没有mysql），所以inception放在数据库所在服务器
```bash
tar xvf inception.tar -C /usr/local/

nohup /usr/local/inception/bin/Inception --defaults-file=/usr/local/inception/bin/inc.cnf &
```


```
#让mySQl的自增id从1开始的方法
book 为表名，truncate table book;


#建库
mysql> create database aud2;

#建表
create table book(id int(8) not null primary key auto_increment,
                  name varchar(45),
                  price float)engine=INNODB auto_increment=1;

#插入数据
INSERT INTO book(id, name, price) VALUES (1,'good',12);

#不带id也可以
INSERT INTO book(id, name, price) VALUES ('jump',15);


#查看数据
mysql> select * from book;
+----+------+-------+
| id | name | price |
+----+------+-------+
|  1 | good |    12 |
|  2 | jump |    15 |
+----+------+-------+


#删除数据
DELETE FROM book WHERE id = 1;


mysql -uroot -h192.168.56.138 -P6669

mysql> inception get variables;


nohup /usr/local/inception/bin/Inception --defaults-file=/usr/local/inception/bin/inc.cnf &


GRANT ALL PRIVILEGES ON *.* TO root@"192.168.56.138" IDENTIFIED BY "123456";


docker run -d -e HOST=192.168.56.138 -e MYSQL_ADDR=192.168.56.10 -e MYSQL_USER=root -e MYSQL_PASSWORD=123456 -p8080:80 -p8000:8000 registry.cn-hangzhou.aliyuncs.com/cookie/yearning:v1.3.3
```
