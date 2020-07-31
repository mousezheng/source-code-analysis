---
title: other
tags: 新建,模板
renderNumberedHeading: true
grammar_cjkRuby: true
---

## helper 与 util 的区别

- 前者主要用于某个类或者某个功能的协助，使用范围较小，仅用于服务某个类，或者由某个类拆分出的小类。后者则更加通用，可以使用到任何地方，方法以**静态方法**为主。
  
 ## Cookie
 
 ### 属性

| 名称 | 描述 |
|-|-|
|$name|    名称|
|$value   | 值|
|$expire |  过期时间|
|$path  |   服务器存储路径|
|$domain  | 所属域名|
|$secure  | cookie 仅仅适用于 https 请求|
|$httpOnly| 仅在 http 协议中使用，用户可通过 JS 访问|
|$raw   |   不需要通过 url 编码发送|
|$sameSite | 允许其他域名使用 |
