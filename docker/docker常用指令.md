一、查看docker信息（version、info）
```
# 查看docker版本 
docker version
 
# 显示docker系统的信息 
docker info
```

二、对image的操作（search、pull、images、rmi、history）
```
https://hub.docker.com/r/library/mysql/tags/  可以在这里面去查找对应的镜像的标签

# 检索image 
docker search centos    ( 在线查找centos的镜像)
 
# 下载image 
docker pull <镜像名:tag>
如
docker pull centos:centos6.6
docker pull centos:6.6

#列出镜像列表; 
docker images
 
#删除单个images，通过image的id来指定删除谁
docker rmi <image id>

#想要删除untagged images，也就是那些id为<None>的image的话可以用
docker rmi $(docker images | grep none | awk '{print $3}' | sort -r)
docker rmi $(docker images | grep "^<none>" | awk "{print $3}")

#要删除全部image的话
docker rmi $(docker images -q)
docker rmi -f $(docker images -q)
```

三、运行容器
```
#运行hello-world的容器
docker run hello-world

#在容器中运行"echo"命令，输出"hello word"
docker run centos:6.6 echo "hello word"

docker run -it centos:6.6 /bin/bash
 
#在容器中安装新的程序 
docker run centos:6.6 yum install vim -y

#交互式进入容器中
docker run -i -t centos:centos6.6 /bin/bash
 
#再一次进刚才进入的容器
docker attach [容器ID]
docker exec -it [容器ID]  /bin/bash   建议使用

#不建议使用attach,它有一个缺点，只要这个连接终止，或者使用了exit命令，容器就会退出后台运行

docker run --name redmine -p 9003:80 -p 9023:22 -d -v /var/redmine/files:/redmine/files -v /var/redmine/mysql:/var/lib/mysql sameersbn/redmine

#其中，-t 选项让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上， 
      -i 则让容器的标准输入保持打开,在交互模式下，用户可以通过所创建的终端来输入命令
      -p 宿主机的9003端口映射到容器的80端口
      -v 数据卷挂载
      -d 后台运行
```
四、查看容器
```
# 列出当前所有正在运行的container 
docker ps
 
# 列出所有的container 
docker ps -a 
 
# 列出最近一次启动的container 
docker ps -l
```
五、对容器的操作（rm、stop、start、kill、logs、diff、top、cp、restart、attach)
```
# 停止所有的container，这样才能够删除其中的images：
docker stop $(docker ps -a -q)

# 删除所有容器 
docker rm -f `docker ps -a -q` 

# 停止、启动、杀死一个容器 
docker stop Name/ID
docker start Name/ID
docker kill Name/ID

# 删除单个容器; -f, --force=false; -l, --link=false Remove the specified link and not the underlying container; -v, --volumes=false Remove the volumes associated to the container 
docker rm Name/ID

# 从一个容器中取日志; -f, --follow=false Follow log output; -t, --timestamps=false Show timestamps 
docker logs Name/ID
    
# 列出一个容器里面被改变的文件或者目录，list列表会显示出三种事件，A 增加的，D 删除的，C 被改变的 
docker diff Name/ID
    
# 显示一个运行的容器里面的进程信息 
docker top Name/ID
    
# 从容器里面拷贝文件/目录到本地一个路径 
docker cp Name:/container_path   to_path 
docker cp ID:/container_path   to_path 
    
# 重启一个正在运行的容器; -t, --time=10 Number of seconds to try to stop for before killing the container, Default=10 
docker restart Name/ID
    
# 附加到一个运行的容器上面; --no-stdin=false Do not attach stdin; --sig-proxy=true Proxify all received signal to the process 
docker attach ID
```

六、查看容器

    docker inspect ID
    
参考文档: 
http://www.jb51.net/article/104679.htm

http://blog.csdn.net/wsscy2004/article/details/25878363
