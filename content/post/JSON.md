---
title: "Json"
date: 2016-07-07T16:43:15
categories: ["Web"]
tags: ["json"]
keywords: []
author: "Canux"
draft: false
---

# JSON

<http://www.json.org/>

JSON: JavaScript Object Notation.

json有两种数据结构.

# key-value/键值对

    {
        key: value,
        key1: value1,
        ...
    }

# 列表/数组

    [value, value1, ...]

# 数据类型

bool:

    python -> True/False
    go -> true/false

string:

    > 只能用双引号.

    python -> str
    go -> string
    go -> []byte -> base64编码字符串

number:

    python -> int/float
    go -> int64/float64

null:

    python -> None
    go -> nil

array

    python -> tuple/list
    go -> array/slice

object

    python -> dict
    go -> struct/map
