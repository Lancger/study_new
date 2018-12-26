# 一、django自定义用户模型
```
ALLOWED_HOSTS = ['*']

#重载系统的用户，让UserProfile生效
AUTH_USER_MODEL = 'accounts.UserProfile'

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'apps.accounts',
    'apps.content',
    'apps.dashboard'
]
```
