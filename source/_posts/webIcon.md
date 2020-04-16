---
title: 网页ICON 优化
date: 2016-01-22 10:49:33
tags:
- css
categories:
- CSS

---
> **Css Sprite**




![Alt text](https://app.yinxiang.com/shard/s72/res/d1f9917c-1bac-4b65-9d48-0d1155ef9a44)

> **Icon Font **

![Alt text](https://app.yinxiang.com/shard/s72/res/669e5e00-8c8b-4e44-853e-aba7ef3fc499)



![Alt text](https://app.yinxiang.com/shard/s72/res/21a4616f-85ba-4514-8878-392a0b6cabc0)

![Alt text](https://app.yinxiang.com/shard/s72/res/961b8eda-7fb8-47eb-b667-957ababb813a)

![Alt text](https://app.yinxiang.com/shard/s72/res/aee97199-8074-47d2-8b9b-ef26c0bc0e97)


<!-- more -->

> icomoon.io字体引入
```stylus
font-face
  font-family: 'music-icon'
  src: url('../fonts/music-icon.eot?2qevqt')
  src: url('../fonts/music-icon.eot?2qevqt#iefix') format('embedded-opentype'),
       url('../fonts/music-icon.ttf?2qevqt') format('truetype'),
       url('../fonts/music-icon.woff?2qevqt') format('woff'),
       url('../fonts/music-icon.svg?2qevqt#music-icon') format('svg')
  font-weight: normal
  font-style: normal
  //匹配class中以icon-开头的  和   class中包含icon-的所有样式
[class^="icon-"], [class*=" icon-"]
  /* use !important to prevent issues with browser extensions that change fonts */
  font-family: 'music-icon' !important
  speak: none
  font-style: normal
  font-weight: normal
  font-variant: normal
  text-transform: none
  line-height: 1

  /* Better Font Rendering =========== */
  // 用于webkit引擎中设置字体的抗锯齿，光滑度属性
  -webkit-font-smoothing: antialiased
  // 用于mac os系统中 火狐浏览器 抗锯齿属性
  -moz-osx-font-smoothing: grayscale
```
![Alt text](https://app.yinxiang.com/shard/s72/res/396d9f7d-b3a4-4353-957f-672458711e45)

![Alt text](https://app.yinxiang.com/shard/s72/res/88df8185-e4bf-4dd8-acdb-e042d6338295)


>### 注意：由于使用icomoon 国外网站慢   so 可以使用 [阿里巴巴矢量图库](http://www.iconfont.cn/collections)
