# 一、项目使用mysql驱动
```
vim workflow/settings.py

# DATABASES = {
#     'default': {
#         'ENGINE': 'django.db.backends.sqlite3',
#         'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
#     }
# }

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'workflow',
        'USER': 'root',
        'PASSWORD': '123456',
        'HOST': '127.0.0.1',
        'PORT': '3306',
        "OPTIONS": {"init_command": "SET default_storage_engine=INNODB;"}
    }
}
```

# 二、模块选择

1、使用pymysql
```
pip3 install -i https://pypi.mirrors.ustc.edu.cn/simple/ pymysql

###在工程目录中的__init__.py中添加如下代码
vim workflow/__init__.py

import pymysql
pymysql.install_as_MySQLdb()
```

2、使用mysqlclient
```
pip3 install -i https://pypi.mirrors.ustc.edu.cn/simple/ mysqlclient
```

# 三、初始化数据
```
# 报错一：

    parent = self.check_key(parent, key[0])
  File "/Users/Lancger/demo/lib/python3.6/site-packages/django/db/migrations/loader.py", line 173, in check_key
    raise ValueError("Dependency on app with no migrations: %s" % key[0])
ValueError: Dependency on app with no migrations: accounts

解决办法：
上面这个报错是因为没有执行数据库同步操作

# 执行数据库同步操作

python manage.py makemigrations

python manage.py migrate

```

# 四、创建用户
```
1、创建超级管理员
python manage.py createsuperuser

2、修改密码
python manage.py changepassword

3、清除sessions
python manage.py clearsessions
```

详情参考：  https://my.oschina.net/u/3138954/blog/1476562


