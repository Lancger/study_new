# 一、自定义过滤器
```
vim workflow/apps/accounts/filters.py
```
```
# -*- coding: utf-8 -*-
__author__ = 'Bryan'


from django_filters import rest_framework as filters
from .models import UserProfile

class AccountsFilter(filters.FilterSet):
    min_age = filters.NumberFilter(field_name="age", lookup_expr='gte')
    max_age = filters.NumberFilter(field_name="age", lookup_expr='lte')   针对age字段过滤

    class Meta:
        model = UserProfile
        fields = ['min_age', 'max_age']
```
# 二、view函数编写
```
vim workflow/apps/accounts/views.py
```
```
# -*- coding: utf-8 -*-
__author__ = 'Bryan'


from apps.accounts.serializers import UserSerializer
from rest_framework import mixins

from django_filters.rest_framework import DjangoFilterBackend
from rest_framework import viewsets

from .models import UserProfile
from .filters import AccountsFilter   ----引入自定义过滤器


# 使用django-filter实现过滤功能
class AccountListViewset(mixins.ListModelMixin, viewsets.GenericViewSet):
    queryset = UserProfile.objects.all()
    serializer_class = UserSerializer
    filter_backends = (DjangoFilterBackend,)
    filter_class = AccountsFilter   --这里引入自定义的过滤函数
```

# 三、示例

  ![filter04自定义过滤示例](https://github.com/Lancger/study_new/blob/master/images/filter03.png)


参考文档：

https://django-filter.readthedocs.io/en/master/guide/rest_framework.html   自定义过滤器
