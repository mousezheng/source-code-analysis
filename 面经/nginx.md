# nginx

## nginx 配置

```jso
...              #全局块

events {         #events块
   ...
}

http      #http块
{
    ...   #http全局块
    server        #server块
    { 
        ...       #server全局块
        location [PATTERN]   #location块
        {
            ...
        }
    }
}
```

- **全局块**：配置影响nginx全局的指令。一般有运行nginx服务器的用户组，nginx进程pid存放路径，日志存放路径，配置文件引入，允许生成worker process数等。
- **events块**：配置影响nginx服务器或与用户的网络连接。有每个进程的最大连接数，选取哪种事件驱动模型处理连接请求，是否允许同时接受多个网路连接，开启多个网络连接序列化等。
- **http块**：可以嵌套多个server，配置代理，缓存，日志定义等绝大多数功能和第三方模块的配置。如文件引入，mime-type定义，日志自定义，是否使用sendfile传输文件，连接超时时间，单连接请求数等。
- **server块**：配置虚拟主机的相关参数，一个http中可以有多个server。
- **location块**：配置请求的路由，以及各种页面的处理情况。

```json
# nginx.conf
user  nginx;       #配置用户或者组，默认为nobody nobody。
worker_processes  1;    #允许生成的进程数，默认为1
error_log  /var/log/nginx/error.log warn; 
#日志路径，级别。这个设置可以放入全局块，http块，server块，级别以此为：debug|info|notice|warn|error|crit|alert|emerg
pid        /var/run/nginx.pid;  #指定nginx进程运行文件存放地址

events {
    accept_mutex on;   #设置网路连接序列化，防止惊群现象发生，默认为on
    multi_accept on;  #设置一个进程是否同时接受多个网络连接，默认为off
    #use epoll;      #事件驱动模型，select|poll|kqueue|epoll|resig|/dev/poll|eventport
    worker_connections  1024;   #最大连接数，默认为512
}

http {
    include       /etc/nginx/mime.types;    #文件扩展名与文件类型映射表
    default_type  application/octet-stream; #默认文件类型，默认为text/plain

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log  main;

    keepalive_timeout  65;
    
    upstream nginx_load_balancing { # 轮训
        server nginx_nginx1_1:80;
        server nginx_nginx2_1:80;
        hash $request_uri;
    }

    server {
        listen  80; #端口监听

        location / {
      	 # 代理
            proxy_pass http://nginx_load_balancing;
        }
    }
	# 导出其他 server 配置
	include /etc/nginx/conf.d/*.conf;  
}
```

## http全局配置

### 错误页面

- error_page 404 /err404.html; #错误页

### 转发ip地址

```json
proxy_set_header Host $host; 
#只要用户在浏览器中访问的域名绑定了 VIP VIP 下面有RS；则就用$host ；host是访问URL中的域名和端口  www.taobao.com:80
proxy_set_header X-Real-IP $remote_addr;  
#把源IP 【$remote_addr,建立HTTP连接header里面的信息】赋值给X-Real-IP;这样在代码中 $X-Real-IP来获取 源IP
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
#在nginx 作为代理服务器时，设置的IP列表，会把经过的机器ip，代理机器ip都记录下来，用 【，】隔开；代码中用 echo $x-forwarded-for |awk -F, '{print $1}' 来作为源IP

```

### 代理配置（未验证）

```json
include       mime.types;   #文件扩展名与文件类型映射表
    default_type  application/octet-stream; #默认文件类型，默认为text/plain
    #access_log off; #取消服务日志    
    log_format myFormat ' $remote_addr–$remote_user [$time_local] $request $status $body_bytes_sent $http_referer $http_user_agent $http_x_forwarded_for'; #自定义格式
    access_log log/access.log myFormat;  #combined为日志格式的默认值
    sendfile on;   #允许sendfile方式传输文件，默认为off，可以在http块，server块，location块。
    sendfile_max_chunk 100k;  #每个进程每次调用传输数量不能大于设定的值，默认为0，即不设上限。
    keepalive_timeout 65;  #连接超时时间，默认为75s，可以在http，server，location块。
    proxy_connect_timeout 1;   #nginx服务器与被代理的服务器建立连接的超时时间，默认60秒
    proxy_read_timeout 1; #nginx服务器想被代理服务器组发出read请求后，等待响应的超时间，默认为60秒。
    proxy_send_timeout 1; #nginx服务器想被代理服务器组发出write请求后，等待响应的超时间，默认为60秒。
    proxy_http_version 1.0 ; #Nginx服务器提供代理服务的http协议版本1.0，1.1，默认设置为1.0版本。
    #proxy_method get;    #支持客户端的请求方法。post/get；
    proxy_ignore_client_abort on;  #客户端断网时，nginx服务器是否终端对被代理服务器的请求。默认为off。
    proxy_ignore_headers "Expires" "Set-Cookie";  #Nginx服务器不处理设置的http相应投中的头域，这里空格隔开可以设置多个。
    proxy_intercept_errors on;    #如果被代理服务器返回的状态码为400或者大于400，设置的error_page配置起作用。默认为off。
    proxy_headers_hash_max_size 1024; #存放http报文头的哈希表容量上限，默认为512个字符。
    proxy_headers_hash_bucket_size 128; #nginx服务器申请存放http报文头的哈希表容量大小。默认为64个字符。
    proxy_next_upstream timeout;  #反向代理upstream中设置的服务器组，出现故障时，被代理服务器返回的状态值。error|timeout|invalid_header|http_500|http_502|http_503|http_504|http_404|off
    #proxy_ssl_session_reuse on; 默认为on，如果我们在错误日志中发现“SSL3_GET_FINSHED:digest check failed”的情况时，可以将该指令设置为off。
```

## 负载均衡

主要在 **http 全局块**中配置，有以下几种方式

### 轮训

```json
upstream nginx_load_balancing {
  	server nginx1_1:80;
  	server nginx2_1:80;
}
```

### 权重

```json
upstream nginx_load_balancing {
  	server nginx1_1:80  weight=1;
  	server nginx2_1:80  weight=2;
}
```

### IP Hash

```json
upstream nginx_load_balancing {
  	ip_hash;
  	server nginx1_1:80;
  	server nginx2_1:80;
}
```

### 最少链接

```json
upstream nginx_load_balancing {
  	least_conn;
  	server nginx1_1:80;
  	server nginx2_1:80;
}
```

### URL Hash

```json
upstream nginx_load_balancing {
  	hash $request_uri;
  	server nginx1_1:80;
  	server nginx2_1:80;
}
```


### 公平最短响应时间（非官方需要装扩展）

```json
upstream nginx_load_balancing {
  	fair;
  	server nginx1_1:80;
  	server nginx2_1:80;
}
```

## location 配置

   ```json
    location [=|~|~*|^~] /uri/ { 
              #… 
    }
   ```

### `=`  精确匹配

```json
location = /nginx {
        try_files $uri /nginx.html;
}
```

```shell
curl http://127.0.0.1:8090/nginx
```

### `^~` 以XX开头 

```json
location ^~ /nginx {
        try_files $uri /nginx.html;
}
```

```shell
curl http://127.0.0.1:8090/nginx/index.html
```

### `~`  区分大小写匹配（正则匹配）

```json

location ~ \.(html | txt)$ {
        try_files $uri /nginx.html;
}
```

```shell
curl http://127.0.0.1:8090/nginx.html
```

### `~*` 不区分大小写匹配

```json

location ~ \.(html | txt)$ {
        try_files $uri /nginx.html;
}
```

```shell
curl http://127.0.0.1:8090/nginx.HTML
```

### `/`   所有匹配

可作为保底，防止 404 问题

```json
location / {
        try_files $uri /index.html;
}
```

## log 日志配置

日志配置参考

```json
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log  main;
```

| 变量名 | 变量说明 |
|:----:|:----:|
| $remote_addr | 客户端地址|
| $remote_user | 客户端用户|
| $time_local | 时间/时区|
| $http_host | 请求地址，浏览器中的地址|
| $request | 用来记录请求的url与http协议|
| $status | 用来记录请求状态；成功是200|
| $upstream_status | upstream 状态|
| $body_bytes_sent | 记录发送给客户端文件主体内容大小|
| $http_referer | 用来记录从那个页面链接访问过来的|
| $http_user_agent | 记录客户浏览器的相关信息|
| $ssl_protocol | SSL 协议版本|
| $http_x_forwarded_for | 访问用户的真实 IP 地址|



## 其他

### 转发配置

```json
location / {
        proxy_pass http://www.baidu.com;
}
```

### 重定向

```json
location / {
        rewrite ^/ http://www.baidu.com;
}
```

### 静态资源

nginx 可以默认代理静态资源，try_files 可以理解为默认为文件链接补全

```json
location / {
  	try_files $uri $uri/index.html index.html;
}

# Project/docker/nginx ❯ curl http://127.0.0.1:8090/    
# index

# Project/docker/nginx ❯ curl http://127.0.0.1:8090/temp
# temp index

# Project/docker/nginx ❯ curl http://127.0.0.1:8090/temp/
# temp index

# Project/docker/nginx ❯ curl http://127.0.0.1:8090/temp/index
# err 404
```

