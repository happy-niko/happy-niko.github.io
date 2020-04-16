---
title: Promise 学习笔记（全）
date: 2018-03-13 15:13:23
tags:
- es6
categories:
- EcmaJs6
banner : https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1520935239437&di=385f20daa20a27d7c413376b1814568c&imgtype=0&src=http%3A%2F%2Fpic.baike.soso.com%2Fp%2F20140710%2F20140710213836-955952945.jpg
---

### Promise  学习笔记



### **释义:**
1.用于异步计算

2.一个promise 表示一个现在，将来或用不可能使用的值 ( 可以将异步操作队列化，按照期望的顺序执行，返回符合预期的结果。)

3.可以在对象之间传递和操作promise ，帮助我们处理队列

<!-- more -->

### **异步回调的四个问题:**

1. 嵌套层次很深，容易陷入回调地域 ，很难维护  
2. 无法正常使用 return  和  throw
3. 无法正常检索堆栈信息，因为每一次回调都是系统层面上的新的堆栈 。
4. 在多个回调之间难以建立联系。

### **Promise 结构:**

```javascript
new Promise(
	//执行器 executor
	function (resolve,reject) {
		//一段耗时很长的异步操作
		resolve() ; //数据处理完成
		reject() ; //数据处理出错
	}

)
.then(function A(){
	//成功 下一步
},function B(){
	// 失败，做相应的处理
})
```

###### 1.Promise 是一个代理对象，它和原先要进行的操作并无关系。只是将原来操作放入了`executor`
###### 2.它通过引入一个回调，避免更多的回调



### **Promise三个状态 **

1. `pending` 待定状态
2. 如果调用 `resolve()` 就进入  `fulfilled` 实现 操作成功状态
3. 如果 `reject()` 就进入 `rejected` 被否决状态 操作失败

当 promise 状态发生改变的时候，就会立即触发`.then()`离得响应函数，处理后续步骤

Promise 状态一经改变就不会改变

执行流程图  

![执行流程图](./1520911191668.png)


### **.then()函数**

```javascript
// 假如在.then()的函数里面不返回新的Promise，会怎样？
// https://www.imooc.com/video/16616

console.log('here we go');
new Promise(resolve => {
    setTimeout( () => {
        resolve('hello');
    }, 2000);
})
    .then( value => {
        console.log(value);
        console.log('everyone');
        (function () {
            return new Promise(resolve => {
                setTimeout(() => {
                    console.log('Mr.Laurence');
                    resolve('Merry Xmas');
                }, 2000);
            });
        }());
        return false;
    })
    .then( value => {
        console.log(value + ' world');
    });

结果：
here we go
hello
everyone
false world
Mr.Laurence
```

`.then()`  接受两个函数作为参数，分别代表 `fulfilled` 和 'rejected'

`.then()` 返回一个新的Promise 实例，所以它可以链式调用

当前面的	`Promise` 状态改变时，`.then()` 根据其最终状态，选择特定的状态响应函数执行 。

状态响应函数可以反悔新的`Promise` ，或其它值

如果返回新的 `Promise` ，那么下一级 `.then()` 会在新 Promise 状态改变之后执行

如果返回其它任何值，则会立即执行下一级 `.then()`   


####  .then() 里面有 .then() 的情况

1. 因为 `.then()` 返回的还是Promise 实例

2. 外层的 `.then` 会等到 里面的` .then() `执行完成 再执行

3. 对于我们来书，此时最好将其展开，会更好读

```javascript
// 嵌套.then()
// https://www.imooc.com/video/16618

console.log('start');
new Promise( resolve => {
    console.log('Step 1');
    setTimeout(() => {
        resolve(100);
    }, 1000);
})
    .then( value => {
        return new Promise(resolve => {
            console.log('Step 1-1');
            setTimeout(() => {
                resolve(110);
            }, 1000);
        })
            .then( value => {
                console.log('Step 1-2');
                return value;
            })
            .then( value => {
                console.log('Step 1-3');
                return value;
            });
    })
    .then(value => {
        console.log(value);
        console.log('Step 2');
    });

结果：
start
step 1
step 1-1
step 1-2
step 1-3
110
step 2
```




### **思考：**

以下问题出自：
[原文：We have a problem with promises](https://pouchdb.com/2015/05/18/we-have-a-problem-with-promises.html)
[FEX 中文翻译](http://fex.baidu.com/blog/2015/07/we-have-a-problem-with-promises/)
[慕课网视频讲解](https://www.imooc.com/video/16619/0)



##### 以下四种Promise 的区别是什么呢？
```javascript
doSomething().then(function () {
  return doSomethingElse();
});

doSomething().then(function () {
  doSomethingElse();
});

doSomething().then(doSomethingElse());

doSomething().then(doSomethingElse);
```


##### 答案 ：
```javascript
第一题：
doSomething().then(function () {
  return doSomethingElse();
}).then(finalHandler);

doSomething
|-----------------|
                  doSomethingElse(undefined)
                  |------------------|
                                     finalHandler(resultOfDoSomethingElse)
                                     |------------------|
第二题：
doSomething().then(function () {
  doSomethingElse();
}).then(finalHandler);

doSomething
|-----------------|
                  doSomethingElse(undefined)
                  |------------------|
                  finalHandler(undefined)
                  |------------------|

第三题：

doSomething().then(doSomethingElse())
  .then(finalHandler);


doSomething
|-----------------|
doSomethingElse(undefined)
|---------------------------------|
                  finalHandler(resultOfDoSomething)
                  |------------------|

第四题：
doSomething().then(doSomethingElse)
  .then(finalHandler);

doSomething
|-----------------|
                  doSomethingElse(resultOfDoSomething)
                  |------------------|
                                     finalHandler(resultOfDoSomethingElse)
                                     |------------------|

```




#### 版权说明：以上均为 [慕课网课程：Promise 入门](https://www.imooc.com/learn/949) 学习笔记,如有侵权[联系我](18735443767)删除
