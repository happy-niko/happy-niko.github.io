---
title: Symbol数据类型
date: 2016-10-21 11:01:11
tags:
- es6
categories:
- EcmaJs6
---


> 什么是Symbol 数据类型？

1.  **MDN** 的解释：

		Symbol()函数会返回symbol类型的值，每个从Symbol()返回的symbol值都是唯一的， 一个symbol值能作为对象属性的标识符； 这是该数据类型仅有的目的。

 2.  个人理解：

	  Symbol类型是es6新增的一个数据类型,Es5的基本数据类型（undefined，null,Object,function,Number,string）Symbol值通过Symbol函数生成Symbol类型是保证每个属性的名字都是独一无二的，对于一个对象由对个模块构成的情况非常有用 。

<!-- more -->
>怎么使用?


**值的输出：**
```javascript
var a=Symbol(‘foo’)=>Symbol(foo) //与其他类型不能运算，可以转换成字符串
```
