---
title: vue  导航
date: 2017-05-23 10:57:18
tags:
- vuejs
categories:
- VUE
---

>学习工具
![Alt text](https://app.yinxiang.com/shard/s72/res/61f82848-a4be-4740-bef7-325f4d827e76)

>三个特点

- 响应式-双响数据绑定
- 单文件组件
-
<!-- more -->

>脚手架安装

![Alt text](https://app.yinxiang.com/shard/s72/res/d827d45b-e1b4-485e-8267-c0d68e138443)

1. 当一个 Vue 实例被创建时，它向 Vue 的响应式系统中加入了其 data 对象中能找到的所有的属性。当这些属性的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。**当这些数据改变时，视图会进行重渲染。**

```javascript
// 我们的数据对象
var data = { a: 1 }
// 该对象被加入到一个 Vue 实例中
var vm = new Vue({
  data: data
})
// 他们引用相同的对象！
vm.a === data.a // => true
// 设置属性也会影响到原始数据
vm.a = 2
data.a // => 2
// ... 反之亦然
data.a = 3
vm.a // => 3
```

> 生命周期：每个 Vue 实例在被创建之前都要经过一系列的初始化过程。例如需要设置数据监听、编译模板、挂载实例到 DOM、在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，给予用户机会在一些特定的场景下添加他们自己的代码。

比如 created 钩子可以用来在一个实例被创建之后执行代码：

```javascript
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```
*** 也有一些其它的钩子，在实例生命周期的不同场景下调用，如 mounted、updated、destroyed。钩子的 this 指向调用它的 Vue 实例。***

![Alt text](https://app.yinxiang.com/shard/s72/res/60a600bf-fb6b-4b92-9641-094df5ba02e1)
