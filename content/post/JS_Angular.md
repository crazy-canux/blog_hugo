---
title: "Angular"
date: 2017-03-13T09:36:32
categories: ["Web"]
tags: ["angular"]
keywords: []
author: "Canux"
draft: false
---

# angularjs

angularjs1.*

<https://github.com/angular/angular.js>

<https://angularjs.org/>

***

# Angular

Angular 是一个用 HTML 和 TypeScript 构建客户端应用的平台与框架。

angular.js 的升级版

<https://github.com/angular/angular>

<https://github.com/angular/angular-cli>

<https://angular.io>

<https://angular.cn>

安装 angular-cli:

    $ npm install -g @angular/cli
    
查看版本:

    $ ng v
    
新建项目:

    $ ng new <my-app>

    # 严格模式
    $ ng new <my-app> --strict
    
测试项目:

    $ cd <my-app>
    // --open会自动打开浏览器
    $ ng serve --open
    
打包:

    $ ng build --prod

***

## Module

angular的模块化系统NgModule, 也就是根模块，习惯命名为AppModule,位于app.module.ts文件。

NgModule是一个带有@NgModule()装饰器的类，该装饰器是一个函数.

重要的属性:

* declarations: 可申明对象表，数据本NgModule的组件，指令和管道。
* exports：导出表，能在其它模块的组件模板中使用的可申明对象的子集。
* imports：导入表，导出了本模块中的组件模板所需的类的其它模块。
* providers：本模块向全局服务中贡献的那些服务的创建器。
* bootstrap：应用的主视图，称为根组件，是应用中所有其它视图的宿主。

## conponent

组件控制屏幕上被称为视图的一小片区域, 每个angular应用最少有一个根组件。

组件配置选项：

* selector: css选择器
* templateUrl: 该组件的HTML模板文件相对于组件文件的地址。
* template:
* styleUrls: 
* style:
* providers: 当前组件所需的服务提供者的一个数组。

## template

## directive

## dependency injection
