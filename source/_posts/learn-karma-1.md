---
layout: post
title: "学习Karma(一)"
date: 2018-06-23
comments: true
---

## 什么是Karma

> 
> Karma is essentially a tool which spawns a web server that executes source code against test code for each of the browsers connected. The results of each test against each browser are examined and displayed via the command line to the developer such that they can see which browsers and tests passed or failed.

根据 Karma 主页上所说，Karam 本质上是一个生成 web server 的工具，而不是一个测试框架。 Karma 主要作用是在各个浏览器上测试源码是否正确。

## 如何使用

1. 安装 Karma

```
npm install -g karma-cli
```

2. 初始化测试

```
karma init karma.conf.js
```

```
1. Which testing framework do you want to use ? (mocha)
2. Do you want to use Require.js ? (no)
3. Do you want to capture any browsers automatically ? (Chrome)
4. What is the location of your source and test files ? (https://cdn.bootcss.com/jquery/2.2.4/jquery.js, node_modules/should/should.js, test/**.js)
5. Should any of the files included by the previous patterns be excluded ? ()
6. Do you want Karma to watch all the files and run the tests on change ? (yes)
```


3. 启动测试

```
karma start karma.conf.js
```

如果在终端上看到

```
Chrome 67.0.3396 (Mac OS X 10.13.5): Executed 4 of 4 SUCCESS (2.68 secs / 0.334 secs)
```
表示所有的方法都已经测试通过

如果在终端上看到

```
... // 这里会列出什么方法失败了，可以自己对照查看

Chrome 67.0.3396 (Mac OS X 10.13.5): Executed 4 of 4 (1 FAILED) (2.024 secs / 2.009 secs)
```
表示有方法失败了，需要进行修改。


## 参数介绍

打开 karma.config.js 我们可以看到

```
// Karma configuration
// Generated on Thu Jun 21 2018 22:28:40 GMT+0800 (GMT+08:00)

module.exports = function(config) {
  config.set({

    // base path that will be used to resolve all patterns (eg. files, exclude)
    basePath: '',


    // frameworks to use
    // available frameworks: https://npmjs.org/browse/keyword/karma-adapter
    frameworks: ['mocha'],


    // list of files / patterns to load in the browser
    files: [
      'https://cdn.bootcss.com/jquery/2.2.4/jquery.js', 'node_modules/should/should.js', 
      'test/**.js',
    ],


    // list of files to exclude
    exclude: [
    ],


    // preprocess matching files before serving them to the browser
    // available preprocessors: https://npmjs.org/browse/keyword/karma-preprocessor
    preprocessors: {
    },


    // test results reporter to use
    // possible values: 'dots', 'progress'
    // available reporters: https://npmjs.org/browse/keyword/karma-reporter
    reporters: ['progress'],


    // web server port
    port: 9876,


    // enable / disable colors in the output (reporters and logs)
    colors: true,


    // level of logging
    // possible values: config.LOG_DISABLE || config.LOG_ERROR || config.LOG_WARN || config.LOG_INFO || config.LOG_DEBUG
    logLevel: config.LOG_INFO,


    // enable / disable watching file and executing tests whenever any file changes
    autoWatch: true,


    // start these browsers
    // available browser launchers: https://npmjs.org/browse/keyword/karma-launcher
    browsers: ['Chrome'],


    // Continuous Integration mode
    // if true, Karma captures browsers, runs the tests and exits
    singleRun: false,

    // Concurrency level
    // how many browser should be started simultaneous
    concurrency: Infinity
  })
}

```

1. frameworks： 用到的测试框架。
2. files：可以配置通配符把源代码和测试代码加载进来。
3. browsers：需要启动的浏览器。
4. autoWatch：观察文件是否有变动，如果有变动就自动更新。



---
参考资料： 

1. [使用karma+mocha进行前端测试驱动开发（一）](https://iyaozhen.com/use-karma-and-mocha-for-fe-tdd.html)
2. [Karma](http://karma-runner.github.io/2.0/intro/how-it-works.html)