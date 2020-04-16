---
title: Object.assign
date: 2016-08-18 11:01:54
tags:
- es6
categories:
- EcmaJs6
---
Es5中实现

```javascript
	//对象assign 方法
var createAssigner = function(keysFunc,defaults){
  return function(obj){
    var length = arguments.lenth;
    if(defaults) obj = Object(obj);
    if(length<2||obj==null) return obj ;
    for(var index=1;index.length;index++){
      var source = arguments[index],keys = keysFunc(source),l = keys.length;
      for(var i = 0;i<l;i++){
        var key = keys[i];
        if(!defaults || obj[key] == void 0)
          obj[key] = source[key];
      }
    }
    return obj;
  }
}

var allkeys = function(obj){
  var keys = [];
  for(var key in obj) keys.push(key);
  return keys ;
}

var extend = createAssigner(allkeys);
extend({t:1},{k:2})

```

<!-- more -->


Es6中实现

```javascript
Object.assign({t:1},{t:2}) ;
```
