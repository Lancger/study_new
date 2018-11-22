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
    
    
    注意事项：
    1、inception 需要设置密码，不然会报错 Global environment 
    2、如果使用的容器mysql，需要登录到容器mysql赋权限（可直接通过宿主机3306登录）
    
    #通过宿主机可以连接到容器的mysql（默认docker-compose.yml设置的未这个密码）

    mysql -h192.168.56.138 -uroot -P3306 -pyearning
    
    GRANT ALL PRIVILEGES ON *.* TO root@"%" IDENTIFIED BY “123456”;

    mysql> show grants for root@"%";
    mysql> flush privileges;
     

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

# 四、mysql容器时区问题

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
[root@localhost yearning-docker-compose]# cat docker-compose.yml
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

# 五、inc.cnf配置文件
```
[root@localhost yearning-docker-compose]# cat /usr/local/inception/bin/inc.cnf
[inception]
general_log=1
general_log_file=inception.log
port=6669

socket=/tmp/inc.socket
character-set-client-handshake=0
character-set-server=utf8
inception_support_charset=utf8mb4
inception_enable_nullable=1
inception_check_primary_key=1
inception_check_column_comment=1
inception_check_table_comment=1
inception_enable_blob_type=1
inception_check_column_default_value=1
inception_support_charset=utf8
inception_osc_on=on
inception_check_column_default_value=OFF
inception_check_column_comment=OFF
inception_check_table_comment=OFF
inception_enable_identifer_keyword=ON
inception_remote_backup_host = 192.168.56.138
inception_remote_backup_port = 3306
inception_remote_system_user = root
inception_remote_system_password = 123456
```

# 六、Inception OSC功能

```
#配置PT-osc的路径

# which pt-online-schema-change
/usr/bin/pt-online-schema-change

# grep bin_dir /etc/inc.cnf   
inception_osc_bin_dir=/usr/bin/pt-online-schema-change

#设置osc相关选项
# grep osc inc-mysql-osc.py
inception set session inception_osc_recursion_method=none;
inception set session inception_osc_alter_foreign_keys_method=0;
inception set session inception_osc_min_table_size=1;     #设置表大小超过此参数的值时，inception调用osc工具alter表


#查看osc的进度

#查看osc列表
mysql -h127.0.0.1 -P6669

mysql> inception get osc processlist;

#由上面的processlist可以看到正在执行的osc任务对应的SQLSHA1值，然后使用下方命令可以查看指定的一条osc任务进度

mysql> inception get osc_percent '*8388D07BAD14866D80DD45CD1521F4F0D17C20CE';

```

参考文档：

http://blog.itpub.net/27000195/viewspace-2120332/     Inception相关功能学习 

