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

单个测试文件

    from django.test import TestCase
    
多个测试文件

    mkdir tests
    touch test_case1.py
    touch test_case2.py

# 测试

运行测试程序：

默认情况下运行 manage.py test 会创建测试数据库运行testcase，完成后自动销毁测试数据库.
    
    $ python3 manage.py test // 查找test 开头的文件运行里面的 unittest.TestCase的子类.
    $ python3 manage.py test <package> // 运行指️定应用内的测试
    $ python3 manage.py test <module> // 运行指定模块内的测试
    $ python3 manage.py test <module>.tests.MyTestCase // 运行一个指定的testcase
    $ python3 manage.py test <module>.tests.MyTestCase.test_method // 运行一个指定的test method
    
    --keepdb // 保留测试数据库
    --parallel // 并行运行测试。
    
默认测试数据库名称为"test_DATABASES.NAME", 也可以通过TEST来指定.(参考settings.py).
