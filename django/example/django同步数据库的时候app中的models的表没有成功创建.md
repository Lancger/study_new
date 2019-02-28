## 问题描述：

在django中创建了一个app，而且在app中自定义创建了几个数据表，在同步的时候系统自带的表可以成功，但是models中的没有生效，而且进入对应app下的migrations目录，发现为空，应该如何解决呢！

解决方式：
```
python3 manage.py makemigrations --empty managerbook  # managerbook就是你的app名字，此处要写成自己的app名字

python3 manage.py makemigrations   # 再次正常运行生成迁移文件的命令

python3 manage.py migrate  # 同步数据库
```

参考链接：

https://blog.csdn.net/ouyangzhenxin/article/details/81094458
