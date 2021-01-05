---
title: "JS Test"
date: 2017-03-13T09:36:32
categories: ["Web"]
tags: ["javascript"]
keywords: []
author: "Canux"
draft: false
---

# gulp

Automation - gulp is a toolkit that helps you automate painful or time-consuming tasks in your development workflow.

<https://github.com/gulpjs/gulp>

gulp command line

    $ npm install --global gulp-cli
    
gulp for devDependencies

    $ npm install --save-dev gulp
    
create gulpfile.js and test it.

    $ vim gulpfile.js
    $ gulp

# karma

A simple tool that allows you to execute JavaScript code in multiple real browsers.

angularjs 的 test runner.

The main purpose of Karma is to make your test-driven development easy, fast, and fun.

<https://github.com/karma-runner/karma>

karma command line:

    $ npm install -g karma-cli

karma for devDependencies

    $ npm install --save-dev karma
    $ npm install karma-jasmine karma-chrome-launcher jasmine-core --save-dev
    
run karma

    $ karma start

# jasmine

A JavaScript Testing Framework

<https://github.com/jasmine/jasmine>

install jasmine to devDependencies

    $ npm install --save-dev jasmineinit 
    
jasmine in project
    
    $ npx jasmine init
    
set jasmine as test script in package.json

    > "scripts": { "test": "jasmine" } 
    
run test

    $ npm test
