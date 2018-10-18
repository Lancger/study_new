# CentOs7.5搭建Redis-4.0.1 Cluster集群服务

## 一、环境

    VMware版本号：12.0.0
    CentOS版本：CentOS 7.5.1804
    三台虚拟机(IP)：
    192.168.56.11
    192.168.56.12
    192.168.56.13

## 二、注意事项

    #1、安裝 GCC 编译工具 不然会有编译不过的问题
    yum install -y gcc g++ gcc-c++ make
    
    #2、升级所有的包，防止出现版本过久不兼容问题
    yum -y update
    
    #3、关闭防火墙 节点之前需要开放指定端口，为了方便，生产不要禁用
    #centos 6.x
    service iptables stop
    
    #centos 7.x
    systemctl stop firewalld.service
    
## 三、集群搭建

### 安装 Redis

    #1、下载，解压，编译安装
    cd /opt
    wget http://download.redis.io/releases/redis-4.0.1.tar.gz
    tar xzf redis-4.0.1.tar.gz
    cd redis-4.0.1
    make
    
    #2、如果因为上次编译失败，有残留的文件
    make distclean
    
### 创建节点

    #1、首先在 192.168.56.11机器上 /opt/redis-4.0.1 目录下创建 redis-cluster 目录
    mkdir /opt/redis-4.0.1/redis-cluster
    
    #2、在 redis-cluster 目录下，创建名为7000、7001、7002的目录
    cd /opt/redis-4.0.1/redis-cluster
    mkdir 7000 7001 7002
    
    #3、分别修改这三个配置文件，把如下 redis.conf 配置内容粘贴进去
    vi 7000/redis.conf 
    vi 7001/redis.conf
    vi 7002/redis.conf
    
#### redis.conf 配置
    ```bash
    port 7000
    bind 192.168.56.11
    daemonize yes
    pidfile /var/run/redis_7000.pid
    cluster-enabled yes
    cluster-config-file nodes_7000.conf
    cluster-node-timeout 10100
    appendonly yes
   ```
   
#### redis.conf 配置说明

    #端口7000,7001,7002
    port 7000

    #默认ip为127.0.0.1，需要改为其他节点机器可访问的ip，否则创建集群时无法访问对应的端口，无法创建集群
    bind 192.168.252.101

    #redis后台运行
    daemonize yes

    #pidfile文件对应7000，7001，7002
    pidfile /var/run/redis_7000.pid

    #开启集群，把注释#去掉
    cluster-enabled yes

    #集群的配置，配置文件首次启动自动生成 7000，7001，7002          
    cluster-config-file nodes_7000.conf

    #请求超时，默认15秒，可自行设置 
    cluster-node-timeout 10100    

    #aof日志开启，有需要就开启，它会每次写操作都记录一条日志
    appendonly yes
    
    #4、接着在另外两台机器上(192.168.56.12，192.168.56.13)重复以上三步，对应的配置文件的IP修改下即可
    
## 五、启动集群

## 六、检查服务

## 安装 Ruby

## 创建集群

## 关闭集群

## 集群验证

### 连接集群测试
### 检查集群状态
### 列出集群节点

### 打印集群信息

## 集群命令
