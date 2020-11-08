---
title: "Django"
date: 2016-09-20T23:14:07
categories: ["Django"]
tags: ["django"]
keywords: []
author: "Canux"
draft: false
---

# Django

django是python的web框架。

<https://github.com/django/django>

<https://www.djangoproject.com/>

<https://docs.djangoproject.com/zh-hans/3.1/contents/>

django遵守MVC设计模式，采用MTV框架。

M: model,数据存取

T: template，如何展现数据

V: view，展现哪些数据

# 安装

<https://docs.djangoproject.com/zh-hans/3.1/faq/install/#faq-python-version-support>

django1.11是最后一个支持python2.7的长期支持版(2017.4).

django2.0开始只支持python3(2018).

本文以django3.1为例创建名为next的project.

virtualenv中安装：

    $mkdir next
    $cd next
    $virtualenv .venv
    
    # mac/linux
    $source .venv/bin/activate
    
    # windows
    >cd .venv/Scripts
    >activate
    
    $pip install django

验证安装：

    $python
    >>>import django
    >>>django.VERSION
    >>>django.get_version()

安装使用的数据库引擎的库：

    $pip install mysqlclient
    $pip install psycopg2
    $pip install cx_Oracle
    
django默认是mysqlclient，可以使用pymysql替代：

    # vim settings.py
    import pymysql
    pymysql.version_info = (1, 4, 13, "final", 0)
    pymysql.install_as_MySQLdb()

***

# project

创建一个名为next的项目

    $ cd next
    $ django-admin startproject next.

    next
    |-- manage.py
    |-- next
        |- __init__.py
        |- settings.py
        |- urls.py
        |- wsgi.py
        |- asgi.py
        
> next/ 最外层根目录只是你项目的容器， 根目录名称对Django没有影响，你可以将它重命名为任何你喜欢的名称。

> manage.py: 一个让你用各种方式管理 Django 项目的命令行工具。你可以阅读 django-admin and manage.py 获取所有 manage.py 的细节。

> next/ 里层的目录包含你的项目，它是一个纯 Python 包。它的名字就是当你引用它内部任何东西时需要用到的 Python 包名。 (比如 next.urls).

> next/__init__.py：一个空文件，告诉 Python 这个目录应该被认为是一个 Python 包。如果你是 Python 初学者，阅读官方文档中的 更多关于包的知识。

> next/settings.py：Django 项目的配置文件。如果你想知道这个文件是如何工作的，请查看 Django 配置 了解细节。

> next/urls.py：Django 项目的 URL 声明，就像你网站的“目录”。阅读 URL调度器 文档来获取更多关于 URL 的内容。

> next/asgi.py：作为你的项目的运行在 ASGI 兼容的Web服务器上的入口。阅读 如何使用 ASGI 来部署 了解更多细节。

> next/wsgi.py：作为你的项目的运行在 WSGI 兼容的Web服务器上的入口。阅读 如何使用 WSGI 进行部署 了解更多细节。

验证开发服务器：

    $python manage.py runserver
    $python manage.py runserver <ip address>:<port>
    > py manage.py runserver

浏览器输入：

    http://127.0.0.1:8000

## settings.py

默认enable的app:

    INSTALLED_APPS = [
        'django.contrib.admin', //管理员站点， 你很快就会使用它。 
        'django.contrib.auth', //认证授权系统。
        'django.contrib.contenttypes', //内容类型框架。/
        'django.contrib.sessions', //会话框架。 
        'django.contrib.messages', //消息框架。 
        'django.contrib.staticfiles', //管理静态文件的框架。 
    ]
    
    默认开启的某些应用需要至少一个数据表，所以，在使用他们之前需要在数据库中创建一些表
    $ python manage.py migrate
    >py manage.py migrate
    
默认enable的midleware:
    
    MIDDLEWARE = [
        'django.middleware.security.SecurityMiddleware',
        'django.contrib.sessions.middleware.SessionMiddleware',
        'django.middleware.common.CommonMiddleware',
        'django.middleware.csrf.CsrfViewMiddleware',
        'django.contrib.auth.middleware.AuthenticationMiddleware',
        'django.contrib.messages.middleware.MessageMiddleware',
        'django.middleware.clickjacking.XFrameOptionsMiddleware',
    ]
    
支持的template:

    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.jinja2.Jinja2',
            'DIRS': [os.path.join(BASE_DIR, 'templates')],
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    'django.contrib.auth.context_processors.auth',
                    'django.contrib.messages.context_processors.messages',
                ],
            },
        },
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [os.path.join(BASE_DIR, 'templates')],
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    'django.contrib.auth.context_processors.auth',
                    'django.contrib.messages.context_processors.messages',
                ],
            },
        }
    ]

默认的数据库:

    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': BASE_DIR / 'db.sqlite3',
        }
    }

    // 可扩展的数据库
    ENGINE:
        django.db.backends.mysql
        django.db.backends.oracle
        django.db.badkends.postgresql_psycopg2

    NAME:
        your database name

    USER:
        your database username

    PASSWORD:
        your database password

    HOST:
        local database or remote database

    PORT:
        database port

其它配置：

    BASE_DIR = Path(__file__).resolve().parent.parent
    
    SECRET_KEY = '-%b)79izbio$!(g!0io(he)giaqi1@))fzfq!t3s1g1dzysc(r'
    
    WSGI_APPLICATION = 'next.wsgi.application'
    
    ROOT_URLCONF = 'next.urls'
    
    DEBUG = True # 开发用来调试
    DEBUG = False # 部署之后关闭

    ALLOWED_HOSTS = [] # 设置哪些域名可以访问，优先级高于web服务器，debug=false必须设置
    ALLOWED_HOSTS = [''*''] # 允许所有域名访问

    STATIC_URL = '/static/'
    # static目录存放js/css等静态文件,collectstatic命令用来收集静态文件。

    LANGUAGE_CODE = 'en-us'
    TIME_ZONE = 'UTC'
    USE_I18N = True
    USE_L10N = True
    USE_TZ = True
    
## urls.py

    from django.contrib import admin
    from django.urls import path, include
    
    urlpatterns = [
        path('polls/', include('polls.urls')),
        path('admin/', admin.site.urls),
    ]

在项目的urls添加所有应用的urls，为每个应用独立创建urls，方便管理。

## wsgi.py

Web Server Gateway Interface.

    import os
    from django.core.wsgi import get_wsgi_application

    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'next.settings')
    application = get_wsgi_application()
    django通过wsgi来部署，参考django的deploy。
    
## asgi.py

Asynchronous Server Gateway Interface.

    import os
    from django.core.asgi import get_asgi_application
    
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'next.settings')
    application = get_asgi_application()

***

# application

应用，一个项目可以有多个应用，一个应用可以用到多个项目中。

可以单独打包应用发布到pypi，包名格式django-project，参考python的打包方法。

创建一个名为polls的应用：

    $python manage.py startapp polls
    > py manage.py startapp polls

    polls/
    |- __init__.py
    |- migrations
    |  |- __init__.py
    |- admin.py
    |- apps.py
    |- models.py
    |- tests.py
    |- views.py

> migrations 迁移文件夹

> admin.py admin管理界面

> apps.py

> models.py 模型

> test.py 测试

> views.py 视图

> urls.py 新建的application的url

***

# django-admin & manage.py

    [staticfiles]
        collectstatic # 设置STATIC_ROOT = '/var/www/static/project/'用来收集静态文件
        findstatic
        runserver # 启动django自带的web开发服务器

    [sessions]
        clearsessions

    [auth]
        changepassword
        createsuperuser
        
    [contenttypes]
        remove_stale_contenttypes

    [django]
        check
        compilemessages
        createcachetable
        dbshell # 数据库命令行
        diffsettings
        dumpdata # 导出数据
        flush # 清空数据库
        inspectdb
        loaddata # 导入数据
        makemessages
        makemigrations # 创建迁移文件
        migrate # 执行迁移文件
        sendtestemail
        shell # 项目环境终端
        showmigrations
        sqlflush
        sqlmigrate # 查看迁移文件会执行哪些sql
        sqlsequencereset
        squashmigrations
        startapp
        startproject
        test
        testserver