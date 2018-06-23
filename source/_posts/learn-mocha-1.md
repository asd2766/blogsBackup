---
layout: post
title: "学习 Mocha 测试框架(一)"
date: 2018-06-20
comments: true
---

## Mocha 是什么

> 
> Mocha 是一个Javascript的测试框架，我们可以通过它，可以为Javascript应用添加测试，以保证其质量。

## 如何编写测试用例

> Mocha 的作用是运行测试脚本，那么在写完代码之后，我们需要先编写测试脚本。

现在我们有一个 add.js 的脚本

```
function add(a, b) {
	return a + b;
}

module.exports = add;
```

我们要测试这个脚本的正确性，就需要针对 add.js 来写一个测试脚本。

> 通常，测试脚本与所要测试的源码脚本同名，但是后缀名为.test.js（表示测试）或者.spec.js（表示规格）。比如，add.js的测试脚本名字就是add.test.js。

```
var add = require('add.js');
var assert = require('assert').strict;

describe('add.js的测试', function() {
	it ('1 + 1 应该是等于 2', function() {
		assert.strictEqual(add(1, 2), 2);
	});
});

```

add.test.js 是可以独立运行的

在控制台运行： mocha add.test.js

```
// assert.strictEqual(add(1, 2), 2);
正确情况下输出结果： 

  add的测试
    1) 1 + 1 应该等于2


  0 passing (13ms)
  1 failing

// assert.strictEqual(add(1, 2), 3);  
错误情况下输入结果：

 1) add的测试
     1 + 1 应该等于2:

    AssertionError [ERR_ASSERTION]: Input A expected to strictly equal input B:
	+ expected - actual

	   - 2
	   + 3
    + expected - actual

      -2
      +3

```


#### 现在我们先来回顾下 add.test.js 脚本的代码内容

1. 在测试脚本中，应该包含1个或者多个 `describe`，每个 `describe` 应该包含一个或者多个 `it`。
2. `describe` 表示测试的区域范围，可以理解成是一个 `function` 或者 `class`。它是一个函数，第一个参数表示测试的名称，从上面的例子中可以看到我们执行之后，会在控制台上先输出名称以做区分；第二个参数为执行函数，我们需要做的操作都在这个执行函数里面。
3. `it` 表示一个测试用例，也是一个函数，第一个参数也是表示名称，第二个参数是实际执行的函数。

> 我们其实可以在 `describe` 里面再套 `describe`，一个 `describe` 里面也可以添加多个 `it`	

## Mocha 基本使用
1. 我们可以直接测试单个测试脚本，直接执行 mocha add.test.js
2. 我们也可以测试多个测试脚本，那么我们可以将对应的测试脚本都放到 test 文件夹下面，然后cd 到 test 文件夹后，在控制台执行 mocha test1.js test2.js test3.js
3. 或者我们在上一级目录，直接执行 mocha ， 它会自动去测试 test 文件下下面的测试脚本（会执行 test 文件下所有能执行的 js 脚本）。但是它只会执行 test 第一级目录下的测试用例，更下层的并不会执行到。
4. 如果我们想要执行到 `test` 文件夹更下层的测试脚本，那么我们需要加上 `--recursive`， 这样就会执行到 `test` 文件夹下所有的测试脚本，不论在那一层。

---
参考文章/引用文章

1. [测试框架 Mocha 实例教程 -- 阮一峰](http://www.ruanyifeng.com/blog/2015/12/a-mocha-tutorial-of-examples.html)