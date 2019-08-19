# 一、Habor部署配置
```
export version="v1.8.2"

wget https://storage.googleapis.com/harbor-releases/release-1.8.0/harbor-offline-installer-v1.8.2.tgz

tar xf harbor-offline-installer-${version}.tgz
cd harbor/

vim harbor.cfg
hostname = hub.wow
其他默认(http协议)

./install.sh
安装成功后，可以通过http://hub.wow/访问
```
