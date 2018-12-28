# 使用django的model_to_dict实现

```
# -*- coding:utf-8 -*-
__author__ = 'Bryan'


from django.views.generic.base import View
# from django.views.generic import ListView
from apps.accounts.models import UserProfile, VerifyCode


# 使用model_to_dict实现
class AccountsListView(View):
    def get(self, request):
        # 通过django的view实现用户列表页
        json_list = []
        accounts = UserProfile.objects.all()
        from django.forms.models import model_to_dict
        for account in accounts:
            json_dict = model_to_dict(account)
            json_list.append(json_dict)

        from django.http import HttpResponse
        import json
        return HttpResponse(json.dumps(json_list), content_type="application/json")


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
