# NGINX安全模块

nginx可以通过ngx_http_limit_conn_module和ngx_http_limit_req_module配置来限制ip在同一时间段的访问次数。

ngx_http_limit_conn_module：该模块用于限制每个定义的连接数，特别是单个IP 地址的连接数．使用limit_conn_zone和limit_conn指令。

ngx_http_limit_req_module：用于限制每一个定义连接的处理速率，特别是从一个单一的IP地址的请求的处理速率。使用“泄漏桶”方法进行限制．指令：limit_req_zone和limit_req。

### 一、ngx_http_limit_conn_module：限制单个IP连接数示例：

```http { 
http { 
    limit_conn_zone $binary_remote_addr zone=perip：10m; 
    limit_conn_zone $server_name zone=perserver：10m 
		
    #定义一个名为addr的limit_req_zone用来存储session，大小是10M内存，
    #以$binary_remote_addr 为key,
    #nginx 1.18以后用limit_conn_zone替换了limit_conn,
    #且只能放在http{}代码段．
    ... 

    server { 

        ... 

        limit_conn perip 10;  　　　　#单个客户端ip与服务器的连接数．
        limit_conn perserver 100;　　＃限制与服务器的总连接数

        }
```

可能有几个limit_conn指令,以下配置将限制每个客户端IP与服务器的连接数，同时限制与虚拟服务器的总连接数：

```http { 
http { 
    limit_conn_zone $binary_remote_addr zone=perip：10m; 
    limit_conn_zone $server_name zone=perserver：10m 

    ... 

    server { 

        ... 

        limit_conn perip 10;  　　　　#单个客户端ip与服务器的连接数．
        limit_conn perserver 100;　　＃限制与服务器的总连接数

        }
```

###二、ngx_http_limit_req_module：限制单位时间内单一IP的请求数

```
http {
    limit_req_zone $binary_remote_addr zone=perip:10m rate=1r/s;
    limit_req_zone $server_name zone=perserver:10m rate=10r/s;

    ...
    #定义一个名为one的limit_req_zone用来存储session，大小是10M内存，　　
		#以$binary_remote_addr 为key,限制平均每秒的请求为1个，
　　 #1M能存储16000个状态，rate的值必须为整数.

    server {
 
        ...

            limit_req zone=perip burst=5 nodelay;　　
            #漏桶数为５个．也就是队列数．nodelay:不启用延迟．
            limit_req zone=perserver burst=10;　　　　
            #限制nginx的处理速率为每秒10个
        }    
```

burst说明：

```
举例：rate=20r/s每秒请求数为２０个，漏桶数burst为5个，
			
			#brust的意思就是，如果第1秒、2,3,4秒请求为19个，第5秒的请求为25个是被允许的，可以理解为20+5
　　　#但是如果你第1秒就25个请求，第2秒超过20的请求返回503错误．
　　　＃如果区域存储空间不足，服务器将返回503（服务临时不可用）错误　
　　　＃速率在每秒请求中指定（r/s）。如果需要每秒少于一个请求的速率，则以每分钟的请求（r/m）指定。　
```



http://nginx.org/en/docs/http/ngx_http_limit_req_module.html

http://nginx.org/en/docs/http/ngx_http_limit_conn_module.html

