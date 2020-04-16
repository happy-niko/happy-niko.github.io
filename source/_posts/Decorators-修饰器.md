---
title: Decorators 修饰器
date: 2016-10-17 10:59:00
tags:
- es6
categories:
- EcmaJs6
---
>简介：修饰器是一个函数，用于修饰类的行为（修改 扩展类的功能，修饰器只在类的范围使用）。

 需安装：npm install babel-plugin-transform-decorators-legacy --save-dev

#### 1.限制某个属性 是只读的


```javascript
{
 	let readonly = function(target,name,descriptor){
 		// target :修改的类本身
 		// name ： 属性本身
 		// descriptor ：属性的描述对象
 		descriptor.writable = false ;  //只读效果
 		return descriptor ;
 	}

 	class Test {
 		@readonly  // 修饰器
 		time(){
 			return '2018-01-01' ;
 		}
 	}

 	let test=new Test();


  	test.time=function(){
       console.log('reset time');
  	};

 	console.log(test.time()) ;  //  报错

 }
```

<!-- more -->

#### 2.在类的外面 使用修饰器  （必须在class的前面使用）

```javascript
{
  let typename=function(target,name,descriptor){
    target.myname='hello';
  }

  @typename
  class Test{

  }

  console.log('类修饰符',Test.myname);
  // 第三方库修饰器的js库：core-decorators; npm install core-decorators
}
```

> 埋点系统

```javascript
// 案例1

// 日志系统 ： 传统 将埋点写到业务逻辑中

{
  let log =(type)=>{
    return function(target,name,descriptor){
      let src_method = descriptor.value ;
      descriptor.value =(...arg)=>{
        src_method.apply(target,arg);
        console.info(`log ${type}`);

      }
    }
  }

  class AD{
    @log('show')
    show(){
      console.info('ad is show') ;
    }
    click(){
      console.info('ad is click') ;
    }
  }

  let ad = new AD() ;
  ad.show();
  ad.click();
}

// 好处：埋点抽离出来   
```
