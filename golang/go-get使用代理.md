## 一、polipo
通过设置大部分程序都识别的的环境变量：http_proxy和https_proxy，将请求通过代理服务器以http或https的方式发出去。

### 检测当前环境
```
curl ip.gs
```

 安装
```
# mac下
brew install polipo

# ubuntu
sudo apt-get install polipo

# centos

git clone https://github.com/jech/polipo.git
cd polipo

# make.info需要这个
sudo yum install texinfo
make -sj
sudo make install
sudo mkdir /etc/polipo
echo -e '
proxyAddress = "::0"
socksParentProxy = "localhost:8090"
socksProxyType = socks5
daemonise = true
' > /etc/polipo/config

# run now
sudo polipo

```

 配置
```
重启服务
brew services start polipo

mac下：/usr/local/opt/polipo/homebrew.mxcl.polipo.plist

      <string>socksParentProxy=localhost:1080</string>
      
linux下：/etc/polipo/config

socksParentProxy = "x.x.x.x:1080" 
socksProxyType = socks5
```

使用代理
```
export http_proxy="http://13.229.223.57:8090/"
export https_proxy="http://13.229.223.57:8090/"
```

参考文档：

https://blog.csdn.net/ys5773477/article/details/73929161

https://www.jianshu.com/p/fe933bdfe103  建立基于polipo的代理
