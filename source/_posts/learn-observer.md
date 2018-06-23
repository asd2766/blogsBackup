---
layout: post
title: "学习观察者模式(一)"
date: 2018-06-10
comments: true
---

## 观察者模式

> 观察者模式是定义对象间的一种一对多的依赖关系，当该对象状态发生改变的时候，就会发通知给所有依赖者。

最简单的使用案例其实就是，我们给标签绑定绑定事件的时候，其实就是使用观察者模式。 比如：

```
	<button type="button" onclick="buttonClick">
		按钮点击
	</button>
```

我们在给 button 绑定 onclick 事件，当该按钮触发的点击事件，那么我们就能在 buttonClick 这个方法里面接收到反馈。这是观察者模式最简单的应用。这个过程浏览器已经帮我们处理好了，所以其实我们用了这么久，一直都不知道这其实也是运用了观察者模式。

在我们自己处理逻辑的时候，处理过程可能会相对比较复杂，也会发现很多地方我们可能都是用到了类似的方法，那么我们可以将这个抽象出一个公共的方法来。

比如我们订阅了一个技术博客, 那么当那个技术博客有新的博客发布的时候, 那么就会给所有的订阅者发通知邮件.

```
class EmailList {
	constructor() {
		this.emailList = [];
	}

	add(email) {
		return this.emailList.push(email);
	}

	remove(email) {
		this.emailList = this.emailList.filter(item => item != email);
		return this.emailList;
	}

	count() {
		return this.emailList.length;
	}

	get(index) {
		if (index < 0 || index > this.emailList.length) {
			return '';
		}
		return this.emailList[index]
	}

}

class BlogSite {
	constructor() {
		this.blogEmails = new EmailList();
	}

	// 这里就是我们给博客网站留下我们的email地址来接收博客更新的文章
	addEmail(email) {
		this.blogEmails.add(email);
	}

	// 这个方法是我们退订博客的时候调用的
	removeEmail(email) {
		this.blogEmails.remove(email);
	}

	// 博客更新新文章啦, 要开始给每个订阅者发邮件进行通知
	notify(...args) {
		for (let i = 0; i < this.blogEmails.count(); i++) {
			this.blogEmails.get(i).update(...args);
		}
	}
}
```

## 发布订阅模式
> 发布订阅模式是跟观察者模式很相似的一个模式, 他们最大的区别在于调度的地方

![Alt text](/blogs/img/learnObserver/img1.png)


![Alt text](/blogs/img/learnObserver/img2.png)

>发布订阅模式代码

```
class BlogSite {
	constructor() {
   		this.subscribers = {}; // 相当于调度中心
  	}
  	
  	addEmail(name, email) {
	  	if (!this.subscribers[type]) {
	      this.subscribers[type] = []; // 如果没有该方法,就先注册
	    }
  		this.subscribers[name].push(email);
  	}
  	
  	removeEmail(name, email) {
  		if (!this.subscriber[name] || !this.subscriber[name].length) {
  			// 不存在这个订阅的email
  			return;
  		}
  		
  		this.subcriber[name] = this.subcriber[name].filter(subEmail => subEmail != email);
  	}
  	
  	sendEmail(email, ...args) {
  		// 发送email的具体实现方法
  	}
  	
  	// 博客更新了, 要开始给他们发送邮件了
  	pubshBlog(name, ...args) {
  		if (!this.subscribers[name] 
  		|| !this.subscribers[name].length) {
	      // 不存在这个订阅者, 发布失败
	      return false;
	    }
	    
	    // 开始发布
	    const array = this.subscribers[name];
	    const _this = this;
	    array.forEach(email => {
	      _this.sendEmail(email, ...args);
	    });

    	return this;
  	}
  	
  	
}
```

## 总结
1. 从图中可以看出, 两者的区别, 观察者是有具体的实现类, 而发布订阅模式是有一个统一的调度中心或者说是方法存储中心
2. 两种模式都可以用于松散耦合，可以改进代码管理和潜在的复用(这个我暂时还有点模糊)

---

参考资料:

1.  https://addyosmani.com/resources/essentialjsdesignpatterns/book/#observerpatternjavascript

2.  https://segmentfault.com/a/1190000007248460
3.  https://www.cnblogs.com/lovesong/p/5272752.html
