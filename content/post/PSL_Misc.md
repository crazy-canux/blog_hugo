---
title: "PSL_Misc"
date: 2016-08-15T10:28:46
categories: ["Python"]
tags: ["misc"]
keywords: []
author: "Canux"
draft: false
---

# PSL

Python Standard Library: Python标准库

***

# Internationalization

## gettext

## locale

***

# Program Frameworks

## cmd

## shlex

类shell的词法分析．

classes:

    shlex

functions:

    split(s, comments=False, posix=True)
    # split("command **kwargs") -> ['command', 'arg1', ...]

data:

***

# Custom Python Interpreters

## code

## codeop

***

# Python Language Services

## keyword

    import keyword

functions:

    keyword.iskeyword(keyword) # x.__contains__(y) <==> y in x

data:

    keyword.kwlist # 返回所有关键字的列表

## parser

## ast

## symtable

## symbol

## token

## tokenize

## tabnanny

## pyclbr

## py_compile

## compileall

## dis

## pickletools

***

# Importing Modules

## imp

## importlib

## zipimport

## pkgutil

## modulefinder

## runpy

***

# Miscellaneous Services

## formatter

## ihooks
