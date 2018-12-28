# 一、全局分页
```
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.LimitOffsetPagination',
    'PAGE_SIZE': 1
}
```

# 二、示例

  ![全局分页示例01](https://github.com/Lancger/study_new/blob/master/images/Pagination01.png)


# 二、自定义封装分页函数
```
# -*- coding: utf-8 -*-
__author__ = 'Bryan'


from apps.accounts.serializers import UserSerializer
# from rest_framework.views import APIView
from rest_framework import mixins
from rest_framework import generics
from rest_framework.response import Response
from rest_framework import status
from rest_framework.pagination import PageNumberPagination   ---需要引入这个

from .models import UserProfile

# 自定义分页函数
class StandardResultsSetPagination(PageNumberPagination):
    page_size = 1
    page_size_query_param = 'page_size'
    page_query_param = 'p'   ---自定义分页参数
    max_page_size = 1000

# 使用ListAPIView实现
class AccountListView(generics.ListAPIView):
    queryset = UserProfile.objects.all()
    serializer_class = UserSerializer
    #加载分页函数
    pagination_class = StandardResultsSetPagination

```

# 四、示例

  ![全局分页示例02](https://github.com/Lancger/study_new/blob/master/images/Pagination02.png)


参考资料：

https://www.django-rest-framework.org/api-guide/pagination/
