# 一、ListAPIView源码分析

发现ListAPIView已经继承了mixins.ListModelMixin,GenericAPIView这2个类
```
class ListAPIView(mixins.ListModelMixin,
                  GenericAPIView):
    """
    Concrete view for listing a queryset.
    """
    def get(self, request, *args, **kwargs):
        return self.list(request, *args, **kwargs)
```

