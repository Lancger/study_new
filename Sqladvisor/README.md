# Sqladvisor

## 1、克隆代码
```bash
cd /usr/local/src/
git clone https://github.com/Meituan-Dianping/SQLAdvisor.git
```

## 2、安装依赖
```bash
yum install -y cmake libaio-devel libffi-devel glib2 glib2-devel bison mysql-devel

# 移除mysql-community库(无用途且和Percona-Server有冲突)
yum remove -y mysql-community-client mysql-community-server mysql-community-common mysql-community-libs
yum -y remove mariadb-libs

#yum install http://www.percona.com/downloads/percona-release/redhat/0.1-3/percona-release-0.1-3.noarch.rpm
rpm -Uvh https://www.percona.com/redir/downloads/percona-release/redhat/percona-release-0.1-3.noarch.rpm

yum install Percona-Server-shared-56

cd /usr/lib64/ 
ln -s libperconaserverclient_r.so.18 libperconaserverclient_r.so 

cd /usr/local/src/SQLAdvisor/
cmake -DBUILD_CONFIG=mysql_release -DCMAKE_BUILD_TYPE=debug -DCMAKE_INSTALL_PREFIX=/usr/local/sqlparser ./
make && make install
```

## 3、编译sqladvisor(源码目录)
```bash
cd ./sqladvisor/
cmake -DCMAKE_BUILD_TYPE=debug ./
make
```

## 4、完成测试
```bash
cp /usr/local/src/SQLAdvisor/sqladvisor/sqladvisor /usr/bin/sqladvisor

sqladvisor -h 127.0.0.1  -P 3306  -u root -p '123456' -d test -q "sql语句" -v 1
```

## 5、官方文档学习

https://github.com/Meituan-Dianping/SQLAdvisor/blob/master/doc/QUICK_START.md

https://user.qzone.qq.com/303350019/infocenter
