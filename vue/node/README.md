# CentOS 7下安装 Node.js

## 一、源码安装

### 1、下载源码
    你需要在 https://nodejs.org/en/download/ 下载最新的Nodejs版本，本文v7.8.0以为例:
```bash
yum install gcc gcc-c++
cd /usr/local/src/
wget https://nodejs.org/dist/v7.8.0/node-v7.8.0.tar.gz
```

### 2、解压源码
```bash
tar -zxvf node-v7.8.0.tar.gz
```

### 3、 编译安装
```bash
cd node-v7.8.0
./configure --prefix=/usr/local/node/
make
make install
```

### 4、 配置NODE_HOME，进入profile编辑环境变量
```bash
#vim /etc/profile

#set for nodejs
export NODE_HOME=/usr/local/node/
export PATH=$NODE_HOME/bin:$PATH

:wq保存并退出，编译/etc/profile 使配置生效

source /etc/profile
###验证是否安装配置成功

node -v

输出 v7.8.0 表示配置成功 ###npm模块安装路径

/usr/local/node/lib/node_modules/

注：Nodejs 官网提供了编译好的Linux二进制包，你也可以下载下来直接应用。
```

## 二、编译好的nodejs二进制包
```bash
rm -rf /usr/local/node/
wget https://nodejs.org/dist/v10.15.3/node-v10.15.3-linux-x64.tar.xz
tar -xf node-v10.15.3-linux-x64.tar.xz
mv node-v10.15.3-linux-x64 /usr/local/node

vim /etc/profile
添加
#set for nodejs
export NODE_HOME=/usr/local/node/
export PATH=$NODE_HOME/bin:$PATH

source /etc/profile

root># node -v
v10.15.3

root># npm -v
4.2.0
```

## 三、使用淘宝cnpm

```yaml
#命令提示符执行宝cnpm
npm install cnpm -g --registry=https://registry.npm.taobao.org

cnpm -v 来测试是否成功安装

#通过改变地址来使用淘宝镜像
npm的默认地址是https://registry.npmjs.org/
可以使用 npm config get registry 查看npm的仓库地址
可以使用 npm config set registry https://registry.npm.taobao.org 来改变默认下载地址，达到可以不安装cnpm就能采用淘宝镜像的目的，然后使用上面的get命令查看是否成功
```

## 四、验证nodejs环境是否正常

    参考  http://www.runoob.com/nodejs/nodejs-http-server.html

