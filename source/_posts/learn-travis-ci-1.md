---
layout: post
title: "学习 Travis CI (一)"
date: 2018-06-23
comments: true
---

## 什么是 Travis CI

下面摘自阮一峰的博客
> Travis CI 提供的是持续集成服务（Continuous Integration，简称 CI）。它绑定 Github 上面的项目，只要有新的代码，就会自动抓取。然后，提供一个运行环境，执行测试，完成构建，还能部署到服务器。

> 持续集成指的是只要代码有变更，就自动运行构建和测试，反馈运行结果。确保符合预期以后，再将新代码"集成"到主干。

> 持续集成的好处在于，每次代码的小幅变更，就能看到运行结果，从而不断累积小的变更，而不是在开发周期结束时，一下子合并一大块代码。

## 如何使用 Travis CI

---
参考资料：

1. [阮一峰 持续集成服务 Travis CI 教程](http://www.ruanyifeng.com/blog/2017/12/travis_ci_tutorial.html)
2. [Travis CI 官方教程](https://docs.travis-ci.com/user/gui-and-headless-browsers/)
3. [hexo＋Travis-ci＋github构建自动化博客](https://blog.csdn.net/u012373815/article/details/53574002)