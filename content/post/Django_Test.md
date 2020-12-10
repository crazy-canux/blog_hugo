---
title: "Test"
date: 2016-10-04T04:51:30
categories: ["Django"]
tags: ["test"]
keywords: []
author: "Canux"
draft: false
---

# tests.py

<https://docs.djangoproject.com/zh-hans/3.1/topics/testing/tools/>

<https://docs.djangoproject.com/zh-hans/3.1/topics/testing/advanced/>

单个测试文件

    vim app/tests.py
    from django.test import TestCase
    
多个测试文件

    mkdir -p app/tests
    vi test_case1.py
    vi test_case2.py

# 配置和运行

默认情况下运行 manage.py test 会创建测试数据库运行testcase，完成后自动销毁测试数据库.

测试相关配置

    vim project/settings.py

    DATABASES = {
        'default': {
            'NAME': 'mydb',
            'TEST': {
                // 默认测试数据库是'NAME'前加'test_' (eg: 'test_mydb')
                // 可以通过TEST.NAME指定测试数据库
                'NAME': 'mytestdb'
            }
        }
    }

    # 指定runner
    ## 默认 django.test.runner.DiscoverRunner
    TEST_RUNNER = 'site_main.base_tests.TestRunner
    
    # 指定fixture文件路径
    FIXTURE_DIRS = (os.path.join(BASE_DIR, 'app', 'fixtures'), )

运行测试程序：
    
    $ python3 manage.py test // 查找test 开头的文件运行里面的 unittest.TestCase的子类.
    $ python3 manage.py test <package> // 运行指️定应用内的测试
    $ python3 manage.py test <module> // 运行指定模块内的测试
    $ python3 manage.py test <module>.tests.MyTestCase // 运行一个指定的testcase
    $ python3 manage.py test <module>.tests.MyTestCase.test_method // 运行一个指定的test method
    
    --keepdb // 保留测试数据库
    --parallel // 并行运行测试。
    -v/--verbosity 0/1/2/3  测试输出信息级别，默认为1， 0表示不输出。
    -d/--debug-sql 输出测试执行的sql语句.
    
***

# Client

测试客户端是一个 Python 类，它充当一个虚拟的 Web 浏览器，
允许你测试视图并以编程方式与 Django 驱动的应用程序交互。

    from django.test import Client

***

# 测试Class

## SimpleTestCase

SimpleTestCase继承自unittest.TestCase.

## TransactionTestCase

TransactionTestCase继承自SimpleTestCase.

## LiveServerTestCase

LiveServerTestCase继承自TransactionTestCase.

LiveServerTestCase 和 TransactionTestCase` 的功能基本相同，
但多了一个功能：它在设置时在后台启动一个实时的 Django 服务器，并在关闭时将其关闭。
这就允许使用 Django 虚拟客户端 以外的自动化测试客户端，
例如，Selenium 客户端，在浏览器内执行一系列功能测试，并模拟真实用户的操作。

## TestCase

TestCase继承自TransactionTestCase.

## fixtures

fixtures是类属性, 可以通过模拟在数据库中插入数据进行测试.

在setUp的时候会插入fixtures指定的json文件中的数据。

在tearDown的时候会销毁数据。

    # 导出数据为json格式，用于单元测试
    $ python3 manage.py dumpdata > test_data.json
    
## databases

databases是类属性
