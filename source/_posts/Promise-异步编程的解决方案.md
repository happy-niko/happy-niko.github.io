---
title: Promise 异步编程的解决方案
date: 2016-08-09 11:02:38
tags:
- es6
categories:
- EcmaJs6
---


promise概念：异步编程解决方案

**举例**：函数A执行步骤——》执行完执行 B

 解决上述问题有两种方式：

- 回调方式

![Alt text](https://app.yinxiang.com/shard/s72/res/575b47df-aace-4872-84dd-ef70325114c9)

- 事件触发的方式


<!-- more -->

>使用Promise

```javascript
{
	// ES5  回调解决异步   模拟ajax  
  // 基本定义
  let ajax=function(callback){
    console.log('执行');
    setTimeout(function () {
      callback&&callback.call()
    }, 1000);
  };
  ajax(function(){
    console.log('timeout1');
  })
}

{
  let ajax=function(){
    console.log('执行2');
    return new Promise(function(resolve,reject){
      // resolve:执行下一步操作   reject:终止操作
      setTimeout(function () {
        resolve()
      }, 1000);
    })
  };

  ajax().then(function(){
    console.log('promise','timeout2');
  })
  // 执行步骤 ：
  //    1.运行Ajax方法
  //    2. 运行完之后返回一个Promise 实例，promise实例有resolve，reject方法
}

// 多个任务  

{
  let ajax=function(){
    console.log('执行3');
    return new Promise(function(resolve,reject){
      setTimeout(function () {
        resolve()
      }, 1000);
    })
  };

  ajax()
    .then(function(){
    return new Promise(function(resolve,reject){
      setTimeout(function () {
        resolve()
      }, 2000);
    });
  })
    .then(function(){
    console.log('timeout3');
  })
}

// 多个任务捕获
{
  let ajax=function(num){
    console.log('执行4');
    return new Promise(function(resolve,reject){
      if(num>5){
        resolve();
      }else{
        throw new Error('出错了')
      }
    })
  }

  ajax(6).then(function(){
    console.log('log',6);
  }).catch(function(err){
    console.log('catch',err);
  });

  ajax(3).then(function(){
    console.log('log',3);
  }).catch(function(err){
    console.log('catch',err);
  });
}


{
  // 所有图片加载完 再添加到页面
  function loadImg(src){
    // 此promise实例做图片加载的动作
    return new Promise((resolve,reject)=>{
      let img = document.createElement('img');
      img.src = src ;
      img.onload =function(){
        resolve(img);
      }
      img.onerror=function(err){
        reject(err);
      }
    })
  }

  function showImgs(imgs){
    imgs.forEach(function(img){
      document.body.appendChild(img);
    })
  }


  // Promise.all表示把多个promise实例当做一个来用。当所有promise实例状态发生改变，新的promise实例才会发生变化

  Promise.all([
    loadImg('http://shape-app.oss-cn-beijing.aliyuncs.com/task_alarm/QMkieAh3Jj.png'),
    loadImg('http://shape-app.oss-cn-beijing.aliyuncs.com/activity_test/pAPrTzNMMF.jpg'),
    loadImg('http://shape-app.oss-cn-beijing.aliyuncs.com/activity_test/5kZ7fCeabd.jpg'),
    loadImg('http://shape-app.oss-cn-beijing.aliyuncs.com/activity_test/2QwsHcpWZG.jpg')
  ]).then(showImgs)
}


{
  // 场景：三张不同的图片，位于三个不同的位置，页面需要加载一张图片，不知道三张图片哪个返回快，不关心，但是有三个来源，加载一个就可以，先到先得，哪个回来哪个显示

  // 有一个加载完就显示的到页面
  function loadImg(src){
    // 此promise实例做图片加载的动作
    return new Promise((resolve,reject)=>{
      let img = document.createElement('img');
      img.src = src ;
      img.onload =function(){
        resolve(img);
      }
      img.onerror=function(err){
        reject(err);
      }
    })
  }


  function showImgs(img){
    let p = document.createElement('p');
    p.appendChild(img);
    document.body.appendChild(p);
  }



  // Promise.race  只要数组中有一个状态改变，就会触发新的promise实例。其他就不改变了

   Promise.race([
    loadImg('http://shape-app.oss-cn-beijing.aliyuncs.com/task_alarm/QMkieAh3Jj.png'),
    loadImg('http://shape-app.oss-cn-beijing.aliyuncs.com/activity_test/pAPrTzNMMF.jpg'),
    loadImg('http://shape-app.oss-cn-beijing.aliyuncs.com/activity_test/5kZ7fCeabd.jpg'),
    loadImg('http://shape-app.oss-cn-beijing.aliyuncs.com/activity_test/2QwsHcpWZG.jpg')
  ]).then(showImgs)


}
```
