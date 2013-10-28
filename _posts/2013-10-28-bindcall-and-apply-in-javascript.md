---
layout: post
title: "bind,call and apply in javascript"
description: "翻译——javascript中的bind、call、apply"
category: javascript 
tags: [bind, call, apply]
---
{% include JB/setup %}

_2013_10_28_ 

今天看到一篇文章，关于javascript中的bind、call、apply的，很不错，简单的翻译下
[原文地址](https://variadic.me/posts/2013-10-22-bind-call-and-apply-in-javascript.html)

### Bind, Call and Apply in JavaScript

某天我看到tweet上的一个js片段
	
	var bind = Function.prototype.call.bind(Function.prototype.bind);

第一眼我就知道他是干什么的了，把`x.y(z)`转换为`y(x,z)`，我很高兴的与朋友们分享，但是当他们问我为什么的时候，就傻眼了。

很多很符合编码规范的代码，很容易就知道有什么作用，但是想要解释给那些没有函数式编程经历的人听，就很困难了。

我决定做个简单的例子解释一下。

	// 创建一个简单的对象
	var context = { foo: "bar" };

	// 一个返回调用对象foo属性的函数
	function returnFoo () {
	  returningn this.foo;
	}

	// this指向window,没有foo属性
	returnFoo(); // => undefined

	// 运用bind绑定函数的作用域
	var bound = returnFoo.bind(context);

	// 可以找到foo了~
	bound(); // => "bar"

	//
	// 这就是 Function.prototype.bind 的作用
	// returnFoo是个函数，它继承了Function.prototype.bind
	//

	// 有很多种为函数绑定作用域的方法
	// Call and apply.
	returnFoo.call(context); // => bar
	returnFoo.apply(context); // => bar

	// 顺便说一下，bind是永久改变某个function的作用域
	// bind除了绑定作用域之外，它还可以绑定参数，具体的google之
	// bind是ES5中的方法，很多es5的shim，都是通过判断bind是否存在来按需加载
	//
	// call和apply 则需要每次都调用下call或apply方法

	// 把函数加到这个对象上，也是可以的
	context.returnFoo = returnFoo;
	context.returnFoo(); // => bar

	// 用数组的slice方法讲解下
	// Array.prototype 有一个很有用的方法 slice.
	// 作用是返回一个数组的拷贝，前闭后开
	[1,2,3].slice(0,1); // => [1]

	// 把slice赋值给本地变量
	var slice = Array.prototype.slice;

	// slice 现在是 "unbound".
	// 数组的slice方法作用在给定的作用域或者this上，否则就会不工作
	slice(0, 1); // => TypeError: can't convert undefined to object
	slice([1,2,3], 0, 1); // => TypeError: ...

	// 可以调用call和apply方法给数组增加一个作用域
	slice.call([1,2,3], 0, 1); // => [1]

	// apply跟call作用相似, 不过apply的第二个参数为一个数组
	slice.apply([1,2,3], [0,1]); // => [1]

	// 为slice调用bind
	slice = Function.prototype.call.bind(Array.prototype.slice);

	// slice会把第一个参数用作作用域
	slice([1,2,3], 0, 1); // => [1]

	// 然后把之前的slice换成bind
	var bind = Function.prototype.call.bind(Function.prototype.bind);

	// 用回之前的例子
	var context = { foo: "bar" };
	function returnFoo () {
	  return this.foo;
	}

	// 使用我们全新的bind方法
	var amazing = bind(returnFoo, context);
	amazing(); // => bar

	// Reference: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind


