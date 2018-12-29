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

# 三、GenericAPIView和mixin绑定

  ![组合示例01](https://github.com/Lancger/study_new/blob/master/images/GenericAPIView_and_mixin.png)


# 四、ViewSet和mixin绑定

  ![组合示例01](https://github.com/Lancger/study_new/blob/master/images/viewset_mixin.png)
