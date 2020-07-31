---
title: response.md
tags: 新建,模板,小书匠
renderNumberedHeading: true
grammar_cjkRuby: true
---
## Response

### 属性
- 状态码
- 缓存相关变量
- head
- body
- 编码
- 状态标志
- 版本号

### 方法
|方法名|备注说明|
|-|-|
| _clone |只拷贝 headers|
| _toString |返回 headers、content 等信息|
| create | 直接抛出异常处理|
| prepare |在响应前用于修改响应协议，符合【RFC 2616】规范 |
| sendHeaders | 响应 设置head、cookie、版本信息、状态信息等|

## JsonResponse
添加诸多对数据 Json 格式化的支持。

## StreamedResponse
通过设置回调方法，完成响应。

## BinaryFileResponse
对 prepare 做大量修改，添加 head 内容 Content-Disposition

## RedirectResponse
重写构造方法，通过修改响应码及 body 完成重定向

``` html
<meta http-equiv="refresh" content="0;url='%1$s'" />
```

**StreamedResponse** 与 **BinaryFileResponse** 区别，前者主要是通过设置回到方法，将回调方法的内容打印到 body 中，可以读取文件信息，并将文件信息打印到 body 中，即可完成后者的部分功能。后者较于前者，对于文件支持粒度更细，比如包含文件大小、描述等等。

