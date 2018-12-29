# 一、viewsets原理解析
```
GenericViewSet(viewset)  ---drf
     GenericAPIView      ---drf
          APIView        ---drf
             View        ---django
```


# 二、mixin原理解析
```
mixin
    CreateModelMixin
    ListModelMixin          --集成了list和分页的功能，我们如果不继承，就不能享受到这个功能
    RetrieveModelMixin
    UpdateModelMixin
    DestroyModelMixin
```
