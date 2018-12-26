###一、命令行工具###

django-admin.py是Django的一个用于管理任务的命令行工具，manage.py是对django-admin.py的简单包装，每个Django Project里面都会包含一个manage.py

语法：
```
root># django-admin.py <subcommand> [options]

root># manage.py <subcommand> [options]

subcommand是字命令； options是可选的
```


###二、常用子命令###
```
1、新建一个django项目
django-admin.py startproject project-name


2、新建一个app
python manage.py startapp app-name


3、运行开发服务器
python manage.py runserver 0.0.0.0:8000
或
python manage.py runserver 0:8000


4、同步数据库
python manage.py makemigrations   --生成数据库同步脚本
python manage.py migrate  --同步数据库

注意：Django 老版本可以使用用syncdb
python manage.py syncdb


5、清空数据库
python manage.py flush

此命令会询问是 yes 还是 no, 选择 yes 会把数据全部清空掉，只留下空表。


6、创建超级管理员
python manage.py createsuperuser


7、导出数据、导入数据
python manage.py dumpdata appname > appname.json
python manage.py loaddata appname.json


8、django项目环境终端
python manage.py shell

如果你安装了bpython或者ipython，会自动调用他们的界面


9、数据库执行命令
python manage.py dbshell

django会进行到settings中设置的数据库，如果是mysql或者postgresql，会要求输入用户名和密码
在这个终端可以输入sql语句

10、编译语言文件
python manage.py compilemessages

11、创建语言文件
python manage.py makemessages


```

###三、manage.py特有的一些子命令
```
1、创建超级管理员
python manage.py createsuperuser


2、修改密码
python manage.py changepassword

3、清除sessions
python manage.py clearsessions

```

四、查看帮助
```
1、查看django-admin.py帮助文档
root># django-admin.py help startproject

usage: django-admin.py startproject [-h] [--version] [-v {0,1,2,3}]
                                    [--settings SETTINGS]
                                    [--pythonpath PYTHONPATH] [--traceback]
                                    [--no-color] [--template TEMPLATE]
                                    [--extension EXTENSIONS] [--name FILES]
                                    name [directory]

Creates a Django project directory structure for the given project name in the
current directory or optionally in the given directory.

positional arguments:
  name                  Name of the application or project.
  directory             Optional destination directory

optional arguments:
  -h, --help            show this help message and exit
  --version             show program's version number and exit
  -v {0,1,2,3}, --verbosity {0,1,2,3}
                        Verbosity level; 0=minimal output, 1=normal output,
                        2=verbose output, 3=very verbose output
  --settings SETTINGS   The Python path to a settings module, e.g.
                        "myproject.settings.main". If this isn't provided, the
                        DJANGO_SETTINGS_MODULE environment variable will be
                        used.
  --pythonpath PYTHONPATH
                        A directory to add to the Python path, e.g.
                        "/home/djangoprojects/myproject".
  --traceback           Raise on CommandError exceptions
  --no-color            Don't colorize the command output.
  --template TEMPLATE   The path or URL to load the template from.
  --extension EXTENSIONS, -e EXTENSIONS
                        The file extension(s) to render (default: "py").
                        Separate multiple extensions with commas, or use -e
                        multiple times.
  --name FILES, -n FILES
                        The file name(s) to render. Separate multiple
                        extensions with commas, or use -n multiple times.

2、查看manage.py帮助

root># python manage.py help

Type 'manage.py help <subcommand>' for help on a specific subcommand.

Available subcommands:

[auth]
    changepassword
    createsuperuser

[contenttypes]
    remove_stale_contenttypes

[django]
    check
    compilemessages
    createcachetable
    dbshell
    diffsettings
    dumpdata
    flush
    inspectdb
    loaddata
    makemessages
    makemigrations
    migrate
    sendtestemail
    shell
    showmigrations
    sqlflush
    sqlmigrate
    sqlsequencereset
    squashmigrations
    startapp
    startproject
    test
    testserver

[sessions]
    clearsessions

[staticfiles]
    collectstatic
    findstatic
    runserver
```
