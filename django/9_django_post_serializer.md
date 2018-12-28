# 一、post接口封装
```
vim workflow/apps/accounts/models.py
```
```
# -*- coding: utf-8 -*-
__author__ = 'Bryan'


from apps.accounts.serializers import UserSerializer
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status

from .models import UserProfile

class AccountListView(APIView):
    """
    List all accounts, or create a new account.
    """
    def get(self, request, format=None):
        accounts = UserProfile.objects.all()
        accounts_serializer = UserSerializer(accounts, many=True)
        return Response(accounts_serializer.data)

    def post(self, request, format=None):
        serializer = UserSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```
