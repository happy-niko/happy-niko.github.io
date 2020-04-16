---
title: Iterator 和 for...of 循环
date: 2016-08-23 11:01:54
tags:
- es6
categories:
- EcmaJs6
---


>什么是Iterator接口

用于数组 对象  map set 等数据集合的**统一**读取操作，不同的数据结构，通过for...of这种形式，达到读取的目标，但是背后的iterator接口是不一样的。

>**MDN解释**:
>当需要迭代一个对象的时候（比如在 ``for...of`` 循环的开始时），它的 ``@@iterator`` 方法就会被调用一次（0 个参数），同时返回的迭代器将被用来获取被迭代出来的值。

``for of ``  循环：不断调用iterator接口。


<!-- more -->

##### 示例1：  iterator数组的读取

```javascript
{
	let arr = ['hello','world'];
	let map = arr[Symbol.iterator]();
	console.log(map.next());
	console.log(map.next());
	console.log(map.next());
}
```


上述代码中：

1. 返回结果：

![Alt text](https://app.yinxiang.com/shard/s72/res/b1a5c863-d3d1-4605-9119-afba59b66cf9)

**说明**：结果**返回一个对象**，包含 ``value``  和 ``done`` 两个参数，如果 ``done==false``  说明 循环还有下一步状态，如果没有了，即不再有value ；

>给一个复杂数据结构部署 ``iterator`` 接口,使其可以支持for....of 循环
>另外：还有一种方式：使用generator生成器 详见笔记:  ***Generator生成器***


```javascript
// object 没有部署  iterator 接口，没有iterator 接口，现在要obj 部署iterator 可以用for of 循环  --------自定义iterator接口部署
{
  let obj={
    start:[1,3,2],
    end:[7,9,8],
    [Symbol.iterator](){
      let self=this;
      console.log(this)
      let index=0;

      let arr=self.start.concat(self.end);
      console.log(arr)

      let len=arr.length;
      console.log(len) ;

      return {
        next(){
          if(index<len){
          	console.log("index"+index)
          	console.log(arr[index++])
            return {
              value:arr[index++],// 必须保证index +1 ，进行下一步
              done:false
            }
          }else{
            return {
              value:arr[index++],
              done:true
            }
          }
        }
      }
    }

  }
  for(let key of obj){
  	console.log('***********************')
    console.log(key);
  }
}
```



<b>运行结果：</b>

![Alt text | center ](https://app.yinxiang.com/shard/s72/res/422356a2-06b9-43f1-98d0-e1216c0b33a9)
