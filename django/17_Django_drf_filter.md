# 一、GenericAPIView里面有一个get_queryset方法获取数据
我们可以自己在基于这个添加一些逻辑
GenericAPIView 源码
```
class GenericAPIView(views.APIView):
    """
    Base class for all other generic views.
    """
    # You'll need to either set these attributes,
    # or override `get_queryset()`/`get_serializer_class()`.
    # If you are overriding a view method, it is important that you call
    # `get_queryset()` instead of accessing the `queryset` property directly,
    # as `queryset` will get evaluated only once, and those results are cached
    # for all subsequent requests.
    queryset = None
    serializer_class = None

    # If you want to use object lookups other than pk, set 'lookup_field'.
    # For more complex lookup requirements override `get_object()`.
    lookup_field = 'pk'
    lookup_url_kwarg = None

    # The filter backend classes to use for queryset filtering
    filter_backends = api_settings.DEFAULT_FILTER_BACKENDS

    # The style to use for queryset pagination.
    pagination_class = api_settings.DEFAULT_PAGINATION_CLASS

    def get_queryset(self):
        """
        Get the list of items for this view.
        This must be an iterable, and may be a queryset.
        Defaults to using `self.queryset`.

        This method should always be used rather than accessing `self.queryset`
        directly, as `self.queryset` gets evaluated only once, and those results
        are cached for all subsequent requests.

        You may want to override this if you need to provide different
        querysets depending on the incoming request.

        (Eg. return a list of items that is specific to the user)
        """
        assert self.queryset is not None, (
            "'%s' should either include a `queryset` attribute, "
            "or override the `get_queryset()` method."
            % self.__class__.__name__
        )

        queryset = self.queryset
        if isinstance(queryset, QuerySet):
            # Ensure queryset is re-evaluated on each request.
            queryset = queryset.all()
        return queryset
```
# 二、自定义逻辑返回过滤数据
```
# 自定义过滤数据逻辑
class AccountListViewset(mixins.ListModelMixin, viewsets.GenericViewSet):
    queryset = UserProfile.objects.all()
    serializer_class = UserSerializer

    def get_queryset(self):
        return UserProfile.objects.filter(username="admin")   ---这里过滤username=admin的数据
```
# 三、返回示例

  ![GenericAPIView示例](https://github.com/Lancger/study_new/blob/master/images/filter01.png)


# 四、获取前端传过来的参数过滤判断
```
# 重写get_queryset方法配合前端传递过来的参数查询数据
class AccountListViewset(mixins.ListModelMixin, viewsets.GenericViewSet):
    queryset = UserProfile.objects.all()
    serializer_class = UserSerializer

    def get_queryset(self):
        queryset = UserProfile.objects.all()
        name_args = self.request.query_params.get("name_args", "admin")
        if name_args:
            queryset = queryset.filter(username=name_args)
        return queryset
```
# 五、构造查询
```
http://127.0.0.1:8000/accounts/?name_args=user01

```
# 六、返回数据
