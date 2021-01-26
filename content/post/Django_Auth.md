---
title: "Auth"
date: 2022-01-14T04:29:37
categories: ["Django"]
tags: ["auth"]
keywords: []
author: "Canux"
draft: false
---

# auth.py

自定义authentication backend.

    from django.contrib.auth.backends import BaseBackend
    
    class MyBackend(BaseBackend):
        def authenticate(self, request):
            ...
        def get_user(self, user_id):
            ...
   
配置使用自定义backend
    
    AUTHENTICATION_BACKENDS =  ("apps.ldap_auth.auth.MyBackend",)
    
<https://github.com/etianen/django-python3-ldap>
    
# auth

配置

    INSTALLED_APPS = (
        'django.contrib.auth'，
        'django.contrib.contenttypes'
    )
    
'django.contrib.auth' 包含了验证框架的内核和它的默认模型。

'django.contrib.contenttypes' 是 Django content type system ，允许你创建的模型和权限相关联。
    
    MIDDLEWARE_CLASSES = (
        'django.contrib.sessions.middleware.SessionMiddleware',
        'django.contrib.auth.middleware.AuthenticationMiddleware',
    )
    
用户登录:

    from django.contrib.auth import authenticate, login
    user = authenticate(username='name', password='pw')
    login(request, user)
    
登陆时的默认验证后端:

    AUTHENTICATION_BACKENDS = ['django.contrib.auth.backends.ModelBackend']
    
用户登出:

    from django.contrib.auth import logout
    logout(request)

***
