laravel
==

生命周期
public/index.php
    autoload 加载依赖
    启动 http 请求处理 kernel
    执行 http 请求
        app/http 
            中间件 session、用户权限判断、csrf
            控制器
            业务代码
    结果返回、关闭 kernel
