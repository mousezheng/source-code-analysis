# source-code-analysis
源码剖析

## http-foundation

[http-foundation](https://github.com/symfony/http-foundation) 组件为 HTTP 协议定义了一个面向对象层。

在 PHP 中，请求是由全局变量组成（$_GET, $_POST, $_FILES, $_COOKIE, $_SESSION,......)，响应是由一些方法生成（echo, header(), setcookie()......）。

symfony HttpFoundation 用面向对象层替代了这些全局变量和方法。