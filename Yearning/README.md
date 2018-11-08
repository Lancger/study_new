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


https://oapi.dingtalk.com/robot/send?access_token=b13e1867001b57dcb96fd813417ecd43d72d3ac8b0a0bf33986b87d625917af2


mysql -uroot -h192.168.56.138 -P6669

mysql> inception get variables;


INSERT INTO Persons(PersonID,LastName,City) VALUES (2,'jack','shenzhen');



INSERT INTO Persons(PersonID,LastName,City) VALUES (88,'test','shenzhen');

delete from Persons where PersonID = '2';


delete from persons where PersonID='2';

delete from mysql.user where Host="192.168.56.%";

GRANT ALL PRIVILEGES ON *.* TO root@"192.168.56.138" IDENTIFIED BY "123456";


docker run -d -e HOST=192.168.56.138 -e MYSQL_ADDR=192.168.56.10 -e MYSQL_USER=root -e MYSQL_PASSWORD=123456 -p8080:80 -p8000:8000 registry.cn-hangzhou.aliyuncs.com/cookie/yearning:v1.3.3
```
