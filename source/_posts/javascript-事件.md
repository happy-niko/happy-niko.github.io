---
title: ' javascript 事件'
date: 2017-08-06 14:59:11
tags:
- js
categories:
- Javascript


---


>#### 一、事件的种类



#### 鼠标事件
`click`  `dbclick`  `mouseup`  `mousedown` `mousemove` `mouseover` `mouseenter`  `mouseout` `mouseleave`
#### 滚轮事件
`wheel`
#### 键盘事件
`keydown`  `keypress`  `keyup`

<!-- more -->
#### 进度事件
`abort` `error` `load` `loadstart`  `loadend`  `progress`  `timeout`   
#### 拖拽事件
`drag`  `dragstart`  `dragend`  `dragenter` `dragover` `dragleave`  `drop`
#### 触摸事件
`touchstart`  `touchend`  `toumove`  `touchcancel`
#### 表单事件
`input`  `select`  `change`  `reset` `submit`
#### 文档事件
`load` `error` `pageshow` `pagelive` `DOMContentLoaded` `readystatechange` `scroll` `resize` `copy` `cut` `paste` `focus` `focusin` `focusout`  `blur`  `hashchange` `popstate`






>#### 二、事件传播的三个阶段

##### 1.捕获阶段

从 `window` 对象传导到目标阶段的过程

##### 2.目标阶段

当事件在目标节点出发的过程

##### 3.冒泡阶段

从目标阶段传导回`window`对象的过程

#### 代码示意：



```html
<!DOCTYPE html>
<html>

<head>
 <meta charset="utf-8">
 <title></title>
</head>

<body>

 <div>
   <p>Click Me</p>
 </div>

</body>
<script type="text/javascript">
 var phases = {
   1: 'capture', //捕获阶段
   2: 'target', //当前
   3: 'bubble' //冒泡
 };

 var div = document.querySelector('div');
 var p = document.querySelector('p');

 div.addEventListener('click', callback, true); // 捕获
 p.addEventListener('click', callback, true); //target
 div.addEventListener('click', callback, false); // 冒泡
 p.addEventListener('click', callback, false);  // target

 function callback(event) {
   console.log(event)
   var tag = event.currentTarget.tagName;

   var phase = phases[event.eventPhase];
   console.log("Tag: '" + tag + "'. EventPhase: '" + phase + "'");
 }
</script>

</html>
```

#### 图示：

![Alt text](https://app.yinxiang.com/shard/s72/res/8d53d1e2-c43a-408c-9e3f-a10bef9354ae)


#### <span style="color:red">注意：</span>

##### 用户点击网页的时候，浏览器总是假定click事件的目标节点，就是点击位置的嵌套最深的那个节点（嵌套在`<div>`节点的`<p>`节点）。所以，`<p>`节点的捕获阶段和冒泡阶段，都会显示为target阶段。



>#### 三、事件代理

##### 由于事件会在冒泡阶段向上传播到父节点，因此可以把子节点的监听函数定义在父节点上，由父节点的监听函数统一处理多个子元素的事件。这种方法叫做事件的代理（delegation）。

```html  
 <ul>
   <li></li>
   <li id="lis">
     <p onclick="pClick()">click me</p>
   </li>
   <li></li>
   <li></li>
 </ul>
```

```javascript
 var ul = document.querySelector('ul');
 var lis = document.querySelector('#lis');

 // lis 的监听函数

 lis.addEventListener('click', (event) => {
   console.log('这个是li的第一个监听函数')
   event.stopPropagation()  // 阻止当前事件向上冒泡  ，故 ul监听事件不执行
   event.stopImmediatePropagation()  //  event.stopPropagation只能阻止当前监听事件不冒，但是阻止不了其他监听事件 ，所以使用    event.stopImmediatePropagation

 })
 lis.addEventListener('click', (event) => {
   console.log('这个是li的第二个监听函数')
 })
 // p 元素的监听函数
 function pClick() {

   console.log('这个是P元素的监听函数')
 }
 //   ul监听函数
 ul.addEventListener('click', (event) => {
   console.log('ul的第一个监听函数')
   // 事件代理 处理 子元素 p 的点击
   // if (event.target.tagName.toLowerCase() == "p") {
   //   // p节点触发
   //   console.log('点击了P元素')
   //
   // }

 })


 ul.addEventListener('click', (event) => {
   console.log('ul的第二个监听函数')
 })

```


#### `stopPropagation`  和  `stopImmediatePropagation` 区别 ：

　　在事件处理程序中，每个事件处理程序中间都会有一个event对象，而这个event对象有两个方法，一个是stopPropagation方法，一个是stopImmediatePropagation方法，两个方法只差一个Immediate，这里就说说这两个方法的区别

　　1、`stopImmediatePropagation`方法：

　　　　stopImmediatePropagation方法作用在当前节点以及事件链上的所有后续节点上，目的是在执行完当前事件处理程序之后，停止当前节点以及所有后续节点的事件处理程序的运行

　　2、`stopPropagation`方法

　　　　stopPropagation方法作用在后续节点上，目的在执行完绑定到当前元素上的所有事件处理程序之后，停止执行所有后续节点的事件处理程序

<hr>

　从概念上讲，在调用完stopPropagation函数之后，就会立即停止对后续节点的访问，但是会执行完绑定到当前节点上的所有事件处理程序；而调用stopImmediatePropagation函数之后，除了所有后续节点，绑定到当前元素上的、当前事件处理程序之后的事件处理程序就不会再执行了
















<hr>
#### 版权说明：
阮一峰大神 [javascript标准参考教程](http://javascript.ruanyifeng.com/dom/event.html#toc8)
超载的笨鸟 [浅谈javascript中stopImmediatePropagation函数和stopPropagation函数的区别](http://www.cnblogs.com/dqsBK/p/6287907.html)
