# CentOs7.5搭建Redis-4.0.1 Cluster集群服务

## 一、环境

    VMware版本号：12.0.0
    CentOS版本：CentOS 7.5.1804
    三台虚拟机(IP)：
    192.168.56.11
    192.168.56.12
    192.168.56.13

## 二、注意事项

    #安裝 GCC 编译工具 不然会有编译不过的问题
    yum install -y gcc g++ gcc-c++ make
    
    #升级所有的包，防止出现版本过久不兼容问题
    yum -y update
    
    #关闭防火墙 节点之前需要开放指定端口，为了方便，生产不要禁用
    #centos 6.x
    service iptables stop
    
    #centos 7.x
    systemctl stop firewalld.service
    
## 三、集群搭建

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
