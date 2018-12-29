# 一、安装和配置
```
1、安装插件
pip3 install django-filter

2、配置到setting.py文件中
INSTALLED_APPS = [
    ...
    'django_filters'
]

3、后端配置

REST_FRAMEWORK = {
    'DEFAULT_FILTER_BACKENDS': ('django_filters.rest_framework.DjangoFilterBackend',)
}
```



参考文档：

https://www.django-rest-framework.org/api-guide/filtering/#djangofilterbackend
