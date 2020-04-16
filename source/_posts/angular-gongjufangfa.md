---
title: angular中的工具方法总结
date: 2017-03-02 19:37:43
keywords: zhangyu,javascript,angular.forEach
tags:
- angular
categories:
- Angular
---

##### 工具方法：
***
1. `angular.bind(obj1,obj)` ---改变this指向
	参数方法: `obj1`: 改变后this指向的对象  `obj`: 被改变的对象  

	```javascript
	function show (){
		alert(this);
	}
	show();
	angular.bind(document,show)();  //改变this指向  
	```
<!-- more -->
2. `angular.extend(a,b)` --- 继承方法
	参数方法：`a`:继承者  `b`:被继承
	````javascript
	var a = {
	 	name:'hello'
	 }
	 var b = {
	 	age:'20'
	 }
	 var c = angular.extend(b,a) ;  //将a 给了 b
	 console.log(c); // {name:'hello',age:'20'}
	````

3. `angular.copy(a,b)` --- 拷贝方法
	参数方法：将a的所有值赋给了b，并且取代覆盖了b
	````javascript
	//结构代码同上
	 var c = angular.copy(a,b);  
	 console.log(c); // {name:'hello'}
	 console.log(a); // {name:'hello'}
	 console.log(b); // {name:'hello'}
	````
4. `angular.isArray(需要判断的元素)` --是否为数组

5. `angular.isDate()` --是否为日期对象

6. `angular.isDefined()` --元素是否被定义

7. `angular.isUnDefined()`  --元素是否未被定义

8. `angular.isFunction()` --判断是否为function

9. `angular.isNumber()`

10. `angular.isObject()`

11. `angular.isString()`

12. `angular.isElement(元素)`

13. `angular.version` -- 输出当前使用的angular版本

14. `angular.forEach(obj,fn,result)`
	参数说明：`obj`:被便利的数组或者对象   `fn(value,i)`:遍历后的回调函数  `result`:想要返回的结果

	```javascript
	var a = {
		name:'zhang',
		sex:'boy'
	}
	var result = [] ;

	angular.forEach(a,function(value,i){
		console.log('value:'+value+'\n'+'i:'+i);
		console.log(this) ; //this指的是结果数组
		this.push(value);
	},result)

	console.log(result); // [zhang,boy];
	```

15. `angular.fromJson/toJson` -- 对字符串格式的json 进行解析  和对json进行字符串转换，类似于原生JSON.parse()  JSON.stringify()

	```javascript
	//angular.fromJson()
	var str = '{"name":"hello","age":"18"}' ;
	var json = angular.fromJson(str);
	console.log(json); // {name:hello,age:18}
	//angular.toJson()
	var json = {"name":"hello","age":"18"};
	var str = angular.toJson(json,true);  //true:表示按照格式打印
	console.log(str) // "{"name":"hello","age":"18"}"
	```

16. `angular.lowercase()/uppercase()`  -- 大小写转换

17. `angular.element()` -- angular 中封装的jq方法  具体方法参考[angular文档](https://docs.angularjs.org/api/ng/function/angular.element)
	```javascript
	var oDiv = document.getElementById('div');
	angular.element(oDiv).css('backgroud','red')
	```
	```html
	<div id='div'></div>
	```

18. `angular.bootstrap` --angular 动态初始化angular 相同于 指令`ng-app`
	好处:可以在想要初始化的时候去执行初始化,针对多个angular初始化会同时初始化
	```javascript
	angular.bootstrap(document,['myapp','myapp1'])
	```

19. `angular.injector` -- 在angular内部使用，注册器
