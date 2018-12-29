# 一、配置
```
# -*- coding: utf-8 -*-
__author__ = 'Bryan'


from apps.accounts.serializers import UserSerializer
from rest_framework import mixins


from django_filters.rest_framework import DjangoFilterBackend
from rest_framework import filters
from rest_framework import viewsets

from .models import UserProfile
from .filters import AccountsFilter


# 使用django-filter实现过滤功能
class AccountListViewset(mixins.ListModelMixin, viewsets.GenericViewSet):
    queryset = UserProfile.objects.all()
    serializer_class = UserSerializer
    filter_backends = (DjangoFilterBackend, filters.SearchFilter)   --这里配置使用search
    filter_class = AccountsFilter
    search_fields = ('username', 'age')  --search的字段
    # filter_fields = ('username', )

```
# 二、示例

  ![drf_search示例](https://github.com/Lancger/study_new/blob/master/images/drf_search01.png)

# 三、扩展

```

    '^' Starts-with search.
    '=' Exact matches.
    '@' Full-text search. (Currently only supported Django's MySQL backend.)
    '$' Regex search.

For example:

search_fields = ('=username', '=email')


https://www.django-rest-framework.org/api-guide/filtering/#searchfilter
```
