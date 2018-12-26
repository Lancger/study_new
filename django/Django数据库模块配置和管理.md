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


详情参考：  https://my.oschina.net/u/3138954/blog/1476562


