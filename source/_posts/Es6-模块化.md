---
title: Es6 模块化
date: 2016-09-01 10:57:59
tags:
- es6
categories:
- EcmaJs6
---

> 基本概念

&nbsp;


<!-- more -->
> 语法

```javascript
// A文件
// 作为一个模块   1.导出变量  2.导出函数  3.类暴露出去

//第一种写法
export let A = 123 ;
export function test(argument) {
	// body...
	console.log('test') ;
}

export class Hello{
	test(){
		console.log('class') ;
	}
}
//推荐使用第二种写法
let A = 123 ;
let test = function(){
	console.log('test') ;
}

class Hello{
	test(){
		console.log('class') ;
	}
}

export default {
  A,
  test,
  Hello
}




//B 文件

//第一种写法
import(A,test,Hello) from 'A文件地址' ;

console.log(A,test,Hello) ;

//第二种写法
import * as lesson from './class/lesson17' ;

console.log(lesson.A,lesson.test,lesson.Hello) ;

//推荐使用第三种写法

import lesson17 from './class/lesson17' ;

console.log(lesson17.A,lesson17.test,lesson17.Hello) ;

```
