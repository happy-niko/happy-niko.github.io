---
title: JS日期格式化转换方法
date: 2017-05-06 14:54:44
tags:
  - js
categories:
  - Javascript
---

> ##### 将日期转换为指定的格式：比如转换成 年月日时分秒 这种格式：yyyy-MM-dd hh:mm:ss 或者 yyyy-MM-dd

```
Date.prototype.format = function (fmt) {
  var o = {
    'M+': this.getMonth() + 1, //月份
    'd+': this.getDate(), //日
    'h+': this.getHours(), //小时
    'm+': this.getMinutes(), //分
    's+': this.getSeconds(), //秒
    'q+': Math.floor((this.getMonth() + 3) / 3), //季度
    S: this.getMilliseconds(), //毫秒
  }
  if (/(y+)/.test(fmt)) {
    fmt = fmt.replace(
      RegExp.$1,
      (this.getFullYear() + '').substr(4 - RegExp.$1.length)
    )
  }
  for (var k in o) {
    if (new RegExp('(' + k + ')').test(fmt)) {
      fmt = fmt.replace(
        RegExp.$1,
        RegExp.$1.length == 1 ? o[k] : ('00' + o[k]).substr(('' + o[k]).length)
      )
    }
  }
  return fmt
}

//调用

var time1 = new Date().format('yyyy-MM-dd hh:mm:ss')
console.log(time1)

//或

var time2 = new Date().format('yyyy-MM-dd')
console.log(time2)
```

<!-- more -->

> ##### 将指定的日期转换为"年月日"的格式，代码如下：

```javascript
var oldTime = new Date('2012/12/25 20:11:11').getTime()
var curTime = new Date(oldTime).format('yyyy-MM-dd')
console.log(curTime)
```

> ##### 将 "时间戳" 转换为 "年月日" 的格式.

```javascript
var da = 1402233166999
da = new Date(da)
var year = da.getFullYear() + '年'
var month = da.getMonth() + 1 + '月'
var date = da.getDate() + '日'
console.log([year, month, date].join('-'))
```
