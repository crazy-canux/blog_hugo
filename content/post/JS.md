---
title: "JS"
date: 2017-03-13T09:36:32
categories: ["Web"]
tags: ["javascript"]
keywords: []
author: "Canux"
draft: false
---

# JavaScript概述

Javascript包括三部分:

1. ECMAScript核心,提供核心语言功能．
2. DOM文档对象模型, 提供访问和操作网页内容的方法和接口．
3. BOM浏览器对象模型, 提供与浏览器交互的方法和接口．

***

# javascript基本语法

javascript源程序叫xxx.js.

javascript大小写敏感．

javascript使用驼峰命名法．

javascript的标识符以字母，下划线或美元符号开头，还可以包含数字．不能是关键字．

JvaScript代码块以大括号{}包围,开括号写在第一行结尾处，前面空格，闭括号单独一行。

javascript中所有事物都是对象，都有属性和方法.

JavaScript使用分号;表示一个语句结束, 一般一行写多个语句才需要显示添加分号．

javascript中运算符周围要有空格。

javascript中使用四个空格来缩进。


***

# 注释

单行注释：

    // comment

多行注释：

    /*
     * comment1
     * comment2
     */

***

# 关键字

    # 数据类型
    var function return typeof new delete

    # 流程控制
    if else for in do while switch case default with break continue

    # 修饰符
    void

    # 动作相关
    instanceof this

    # 异常处理
    try catch throw

    # 调试
    debugger

    # ECMAScript2015
    let const

***

# 运算符和优先级

## 算数运算符

## 赋值运算符

## 比较运算符

    // 在比较值钱进行类型转换.
    ==

    // 强制对值和类型进行比较.
    ===

## 逻辑运算符

## 类型运算符

## 位运算符

***

# 数据类型

## var申明变量

通过var申明的变量没有块作用域，在块之外也能访问.

申明变量:

重复申明同名变量不会改变变量的值．

    var varName;
    // 一次申明多个变量
    var varName1, varName2, ...;

赋值:

    varName = varValue;

申明并赋值:

动态类型语言，变量可以赋予不同类型值.

    var varName = varValue;
    // 一次定义多个变量
    var varName1 = varValue1, varName2=varValue2, ...;

申明变量类型:

    var varName = new Boolean/Number/String/Array/Object;

## let申明变量

通过let申明有块作用域的变量.

    {
        let varName = varValue;
    }

## const申明常量

const定义的变量与let相似， 但不能重新赋值.

它没有定义常量值。它定义了对值的常量引用。

    {
        const varName = varValue;
    }

常量对象的属性可以修改：

    const car = {type:"porsche", model:"911", color:"black"};
    car.color = "white";

常量数组元素可以修改：

    const cars = ["audi", "bmw"];
    cars[0] = "honda";

## 布尔/Boolean

布尔类型只有true和false两个值．

    var x = true
    var y = false

## 数字/Number

    var name = 1.3

## 字符串/String

字符串可以用单引号或双引号表示.

    var name = "string"

## Null

Null类型只有一个特殊值null.表示一个空对象指针.

通过null清空变量的值．

    varName = null

## Undefined

没有赋值的变量值为undefined.

Undefined类型只有一个特殊值undefined.

## 对象/Object

对象由大括号包围的键值对表示，中间用逗号隔开.

定义对象：

    var obj = new Object;

    var obj = {key: "val", key1: "val1"};

    var obj = {
        firstName: "bill", // 冒号后空格.
        lastName: "gates" // 最后一个不要都好.
    };  

访问对象属性:

    val = obj.key
    val = obj["key"]

访问对象方法:

    obj.method()

## typeof

该操作符可以返回数据类型:

    undefined
    boolean
    number
    string
    object
    function

***

# 控制流

## if

    if (condition) {
        expression;
    }

    if (condition) {
        expression;
    } else {
        expression;
    }

    if (condition) {
        expression;
    } else if (condition) {
        expression;
    } else {
        expression;
    }

## switch

    switch(expression) {
        case value:
            statement;
            break;
        case value:
            statement;
            break;
        ...
        default:
            statement;
    }

## for

for:

    for (initialization; expression; post-loop-expression) {
        statement;
    }

for-in:

循环遍历对象的属性

    for (property in object) {
        statement;
    }

## while

do-while:

    do {
        statement;
    } while (condition);

while:

    while (condition) {
        statement;
    }

## with

将代码作用于设定到一个特定对象.

    with (expression) {
        statement;
    }

## break和continue

label:

break和continue都可以和label配合使用.

    labelName: statement

break:

跳出循环(for, while).

    labelName:
    for () {
        for () {
            if () {
                break labelName; # 跳出最外层循环
            }
        }
    }

continue:

继续下一次循环.

    labelName:
    for () {
        for () {
            if () {
                continue labelName; # 从外层循环继续循环
            }
        }
    }

***

# 文件和输入输出

## 输出

alert写入警告框：

    window.alert(10);

write写入html：

    document.write(10);

innerHTML写入html:

    document.getElementById("demo").innerHTML = 10;

console.log写入浏览器控制台:

    // 可以在js代码或调试窗口打印变量值：
    console.log(10);

***

# 函数

定义函数：

    // 没参数
    function funcName() {
    }

    // 带参数
    function funcName(args) {
    }

    // 使用变量
    var name = function(args) {};

    // 使用构造方法
    var name = new Function(args, return value);

函数返回值:

    function funcName() {
        return ...
    }

    // 函数返回值赋值给变量
    var value = funcName()

js的函数形式参数和实际参数可以是任意个数和任意类型．

函数内部定义的变量是局部变量,局部变量在函数运行后被删除．

如果把值赋给未申明的变量，该变量被自动作为全局变量．

    name = "value" 相当于全局变量

函数内部属性:

函数内部两个特殊对象arguments和this.

    arguments // 用来存储该函数的所有参数
    arguments.callee // 指针，指向该函数本身.

    this // 表示函数调用语句所处的作用域
    window // 当在全局调用this，引用的就是window对象.

    // 调用函数
    apply()
    call()

***

# 错误和异常

try-catch:

    try {
        statement;
    } catch(err) {
        statement;
    }

throw:

    throw exception

## 严格模式

通过在脚本或函数开头添加严格模式：

严格模式在不声明变量的情况下使用变量，是不允许的

    "use strict";
    ......

## debugger调试

debugger关键字会停止js的执行，如果有调试函数就调用。

如果调试器打开，会在debugger停止执行，如果没有打开调试器，会继续运行。

    var x = 10;
    debugger;
    document.getElementbyId("demo").innerHTML = x;

***

# 模块和包

***

# 文档

***

