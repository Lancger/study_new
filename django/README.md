# 一、创建一个工程
```
cd /opt/

django-admin.py startproject Workflow

```
# 二、创建app
```
cd /opt/Workflow/

django-admin startapp accounts
django-admin startapp content
django-admin startapp dashboard
```

# 三、目录结构
```
(fms) [root@localhost Workflow]# tree  /opt/Workflow/
/opt/Workflow/
├── accounts
│   ├── admin.py
│   ├── apps.py
│   ├── __init__.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── content
│   ├── admin.py
│   ├── apps.py
│   ├── __init__.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── dashboard
│   ├── admin.py
│   ├── apps.py
│   ├── __init__.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── manage.py
└── Workflow
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    └── wsgi.py

7 directories, 26 files
```
