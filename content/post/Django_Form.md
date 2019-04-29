---
title: "Form"
date: 2016-10-04T04:30:02
categories: ["Django"]
tags: ["form"]
keywords: []
author: "Canux"
draft: false
---

# forms.py

    from django import forms

# 表单

用户在浏览器中输入数据提交，对数据验证以及输入框的生成等。

django的表单系统的核心类是django.forms.Form类,所有的构建的表单都是这个类的子类。
