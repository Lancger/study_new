# 一、项目使用mysql驱动
```
vim Workflow/settings.py

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
vim Workflow/settings.py

import pymysql
pymysql.install_as_MySQLdb()
```

2、使用mysqlclient
```
pip3 install -i https://pypi.mirrors.ustc.edu.cn/simple/ mysqlclient
```



详情参考：  https://my.oschina.net/u/3138954/blog/1476562


