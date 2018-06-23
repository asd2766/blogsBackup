---
layout: post
title: "初识 Node Assert"
date: 2018-06-20
comments: true
---

### Assert的作用
---
> assert是 node 里面内中的一个对象, 主要是针对项目进行测试, 如果运行结果跟预期不同, 则会抛出异常.
> 
> node版本为 10.4.1

1. assert(value, [,message]) === assert.ok(value, []),两个方法是一样的
> value: 需要检验的值, 判断是否为true
> message: 抛出异常时的提示信息

2. assert.deepStrictEqual(actual, expected[, message])
> 测试 actual 与 expected 深度是否相同, 深度相等意味着子对象中可枚举的自身属性也会按以下规则递归地比较。 
> 
> 比较的详细说法
> 
> ```
> 1. [原始值][1]运用 SameValue比较法进行比较，使用 Object.is() 函数。
> 2. 对象的类型标签应该相同。
> 3. 对象的原型使用全等运算符比较。
> ```

	例子:
	
  ```javascript
	const assert = require('assert').strict;
	
	assert.deepStrictEqual({ a: 1 }, { a: '1' });
	// 运行上述代码会跑出一个异常, 因为 1 === '1' 是 false
	// throw new errors.AssertionError({
	// AssertionError [ERR_ASSERTION]: Input A expected to strictly deep-equal input B:
	// + expected - actual
  	// {
	// -   a: 1
	// +   a: '1'
 	// }
	
	// 以下对象没有自身属性。
	const date = new Date();
	const object = {};
	const fakeDate = {};
	Object.setPrototypeOf(fakeDate, Date.prototype);
	
	// 原型不同：
	assert.deepStrictEqual(object, fakeDate);
	// 报错
	// AssertionError [ERR_ASSERTION]: Input A expected to strictly deep-equal input B:
	// + expected - actual
	// - {}
	// + Date {}
	// 因为两者原型不同, 不是严格相等
	// 如果将 Object.setPrototypeOf(fakeDate, Date.prototype); 这句删除, 那么两个是相等的
	
	assert.deepStrictEqual(NaN, NaN);
	// 通过，因为使用的是 SameValue 比较法。对应详细说明的1
  ```



--------
参考资料:

[1]: http://www.w3school.com.cn/js/pro_js_value.asp

1. http://nodejs.cn/api/assert.html#assert_assert_value_message

