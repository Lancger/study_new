# 一、使用serializers实现用户数据序列化
```
# -*- coding:utf-8 -*-
__author__ = 'Bryan'


from django.views.generic.base import View
# from django.views.generic import ListView
from apps.accounts.models import UserProfile, VerifyCode


# 使用serializers实现用户数据序列化
class AccountsListView(View):
    def get(self, request):
        # 通过serializers实现用户数据序列化
        accounts = UserProfile.objects.all()

        from django.core import serializers
        from django.http import JsonResponse, HttpResponse
        import json
        json_data = serializers.serialize('json', accounts)
        json_data = json.loads(json_data)

        #In order to allow non-dict objects to be serialized set the safe parameter to False.
        # return JsonResponse(json_data, safe=False)
        return HttpResponse(json_data, content_type="application/json")


# # 使用model_to_dict序列化数据
# class AccountsListView(View):
#     def get(self, request):
#         # 通过model_to_dict实现用户数据序列化
#         json_list = []
#         accounts = UserProfile.objects.all()
#         from django.forms.models import model_to_dict
#         for account in accounts:
#             json_dict = model_to_dict(account)
#             json_list.append(json_dict)
#
#         from django.http import HttpResponse
#         import json
#         return HttpResponse(json.dumps(json_list), content_type="application/json")


# # django_views 实现api_json数据返回
# class AccountsListView(View):
#     def get(self, request):
#         #通过django的view实现用户列表页
#         json_list = []
#         accounts = UserProfile.objects.all()
#         print (accounts)
#
#         for account in accounts:
#             json_dict = {}
#             # 获取用户的每个字段，键值对形式
#             json_dict['name'] = account.name
#             # json_dict['birthday'] = account.birthday   #注意这里日期类型的不支持
#             json_dict['gender'] = account.gender
#             json_dict['mobile'] = account.mobile
#             json_dict['email'] = account.email
#             json_dict['webchat'] = account.webchat
#             json_list.append(json_dict)
#
#         from django.http import HttpResponse
#         import json
#         return HttpResponse(json.dumps(json_list), content_type="application/json")


```

# 二、报错示例返回

  ![serializers_错误示例](https://github.com/Lancger/study_new/blob/master/images/serializers_01.png)
  
  ![serializers_json示例](https://github.com/Lancger/study_new/blob/master/images/serializers_json.png)

  
  
