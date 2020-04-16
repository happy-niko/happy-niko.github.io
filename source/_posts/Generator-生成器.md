---
title: Generator 生成器
date: 2016-08-23 10:59:33
tags:
- es6
categories:
- EcmaJs6
---
>什么是 **Generator**？

解决异步编程 ：
&nbsp;&nbsp;&nbsp;&nbsp; Generator 包含多个步骤，每个步骤包含``yield`` 或者 ``return``  ，遇到就会停止，进行下一步，调用``next()``函数 。

>基本用法：



```javascript
{
	// Generator 基本用法
	let tell=function* (){
    yield 'a';
    yield 'b';
border-left: 5px solid gray;    return 'c'
  	};

  let k=tell();

  console.log(k.next());
  console.log(k.next());
  console.log(k.next());
  console.log(k.next());

}
```

<!-- more -->

> **generator 函数  和  iterator 接口的关系 ：**

>任何一个对象的iterator接口都是部署到Symbol.iterator属性上，generator就是遍历器的生成函数，所以可以直接把generator直接赋值给Symbol.iterator，从而使得对象也具备了iterator接口。

```javascript
{
  let obj={};
  obj[Symbol.iterator]=function* (){
    yield 1;
    yield 2;
    yield 3;
  }

  for(let value of obj){
    console.log('value',value);
  }
}
```

>**状态机**  ： 比如使用A、B、C 描述一个事物三种状态。用Generator 处理特别适用。

```javascript

{
  let state=function* (){
	//当状态为1 的时候 不断next() 可以循环状态
    while(1){
      yield 'A';
      yield 'B';
      yield 'C';
    }
  }
  let status=state();
  console.log(status.next());
  console.log(status.next());
  console.log(status.next());
  console.log(status.next());
  console.log(status.next());
}
```
**结果：**

![Alt text | center](https://app.yinxiang.com/shard/s72/res/326cd889-605a-48b8-8504-019b36da5092)

>async 语法




#### 实例1：抽奖次数限制

```javascript
{

	// 抽奖逻辑 和 次数限制  分离
	let draw = function(count){
		//具体逻辑
		console.info(`剩余${count}次数`);
	}

	draw(1)

	// 计算当前的次数   之前使用全局变量保存当前次数，这样会非常不安全  

	// generator

	let residue = function* (count){
		while(count>0){ // 次数限制限制
			count -- ;
			yield draw(count) ; //抽奖逻辑
		}
	}

	let star = residue (5) ;
	let btn = document.createElement('button') ;


	btn.id = 'start' ;
	btn.textContent = "抽奖" ;
	document.body.appendChild(btn) ;

	document.getElementById('start').addEventListener('click',function(){
		star.next() ;
	},false)
}
```


**结果：**

![Alt text | center](https://app.yinxiang.com/shard/s72/res/1e0566ee-6bb8-4d67-9e1c-cb73b82ebfa3)

#### 实例2：长轮询的generator实现

```javascript
// 实例2   长轮询 ： 服务端数据 定期变化，前端 需要定时去取。之前 通过定时器不断的取  现在使用 generator

{
	let ajax = function* (){
		yield new Promise(function(resolve,reject){
			setTimeout(function(){
				resolve({code:0})
			},200) ;
		})
	}

	// 长轮询

	let pull = function(){
		var generator = ajax() ;
		let step = generator.next() ;
		step.value.then(function(d){
			console.log(d) ;
			if(d.code !=0){
				setTimeout(function(d){
					console.log('wait') ;
					pull() ;
				},1000)
			}else{
				console.log(d) ;
			}
		})
	}

	pull() ;
}

```
