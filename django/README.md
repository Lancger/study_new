# 一、激活虚拟环境
```
#升级pip
wget --no-check-certificate https://bootstrap.pypa.io/get-pip.py
python3 get-pip.py
pip3 install --upgrade pip

## 这里使用虚拟环境(loonflow虚拟环境)
cd /usr/local/
python3 -m venv Workflow
cd Workflow
source /usr/local/Workflow/bin/activate

#安装django
pip3 install -i https://pypi.mirrors.ustc.edu.cn/simple/ django

#安装requirements依赖
cd /opt/Workflow
pip3 install -i https://pypi.mirrors.ustc.edu.cn/simple/  -r requirements/dev.txt
```
# 二、创建一个工程
```
cd /opt/

django-admin.py startproject Workflow

```
# 三、创建app
```
cd /opt/Workflow/

django-admin startapp accounts
django-admin startapp content
django-admin startapp dashboard
```

# 四、目录结构
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

# 五、运行项目
```
(Workflow) [root@localhost Workflow]# python manage.py runserver 192.168.56.138:8000
```

# 六、访问项目

  ![django创建项目01](https://github.com/Lancger/opslinux/blob/master/images/django-init.png)
