## nginx反向代理汇总
### proxy_pass和upstream
1. 用来设置将请求转发给的后端服务器的主机，可以是主机名、IP地址：端口的方式，也可以代理到通过upstream设置的主机组
~~~
upstream webserver {
        #ip_hash;  
        server  192.168.0.201 weight=1 max_fails=2  fail_timeout=2;
        server  192.168.0.202 weight=1 max_fails=2  fail_timeout=2;
        server 127.0.0.1:9008 backup;
}

server {
        server_name  hfnginx.chinacloudapp.cn;
        #access_log  logs/host.access.log  main;
        location / {  #静态网页在本机
            root   html;
            index  index.html;
        }
        location ~* ^/form {  #指定目录在后端服务器
            proxy_pass  http://webserver; #此处http://webserver后面不能加/，如果加了会提示语法错误
            proxy_set_header X-Real-IP $remote_addr;
        }
}
~~~
2. weight（权重）
指定轮询几率，weight和訪问比率成正比，用于后端服务器性能不均的情况。例如以下所看到的。10.0.0.88的訪问比率要比10.0.0.77的訪问比率高一倍。
~~~
upstream linuxidc{ 
      server 10.0.0.77 weight=5; 
      server 10.0.0.88 weight=10; 
}
~~~
3. ip_hash（訪问ip）每一个请求按訪问ip的hash结果分配。这样每一个訪客固定訪问一个后端服务器，能够解决session的问题。
~~~
upstream favresin{ 
      ip_hash; 
      server 10.0.0.10:8080; 
      server 10.0.0.11:8080; 
}
~~~
4. fair（第三方）按后端服务器的响应时间来分配请求。响应时间短的优先分配。与weight分配策略相似。
~~~
 upstream favresin{      
      server 10.0.0.10:8080; 
      server 10.0.0.11:8080; 
      fair; 
}
~~~
5. url_hash（第三方）按訪问url的hash结果来分配请求，使每一个url定向到同一个后端服务器。后端服务器为缓存时比較有效。
注意：在upstream中加入hash语句。server语句中不能写入weight等其他的參数，hash_method是使用的hash算法。
~~~
 upstream resinserver{ 
      server 10.0.0.10:7777; 
      server 10.0.0.11:8888; 
      hash $request_uri; 
      hash_method crc32; 
}
~~~
6. upstream还能够为每一个设备设置状态值，这些状态值的含义分别例如以下：
* down 表示当前的server临时不參与负载.
* weight 默觉得1.weight越大，负载的权重就越大。
* max_fails ：同意请求失败的次数默觉得1.当超过最大次数时，返回proxy_next_upstream 模块定义的错误.
* fail_timeout : max_fails次失败后。暂停的时间。
* backup： 其他全部的非backup机器down或者忙的时候，请求backup机器。所以这台机器压力会最轻。
### proxy_net_upstream
1. 当使用了upstream的时候，可以定义在发生了特定的情况下将请求依次交给下一个组内的服务器处理，状态包括：
~~~
    proxy_next_upstream  http_404 http_502;  //让404报错进入max_fails计数 
    upstream online { 
        sticky; 
        server 172.28.70.161:8080  max_fails=0 fail_timeout=3s ; 
        server 172.28.70.163:8080  max_fails=0 fail_timeout=3s ; 
     
        check interval=3000 rise=2 fall=1 timeout=1000 type=http; 
        check_http_send "GET / HTTP/1.0\r\n\r\n"; 
        check_http_expect_alive http_2xx http_3xx; 
    } 
 
    upstream backup { 
        server 172.28.22.29:7777  max_fails=0 fail_timeout=3s; 
    } 
~~~
### proxy_set_header 机制可修改默认配置及其它头域的值。
~~~
location /some/path/ {
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_pass http://localhost:8000;
}
~~~
如果想阻止某个头域被传递给被代理服务器，可以如下设置头域的值为空
~~~
location /some/path/ {
proxy_set_header Accept-Encoding "";
proxy_pass http://localhost:8000;
}
~~~
### proxy_buffer
* buffer ，即缓冲区，它在 Nginx 上发挥的作用就是 启用一个缓冲区，先在这个缓冲区内进行存储，再把数据发送出去 。和在线观看视频有点类似，先把视频文件缓冲一部分到本地再开始播放。
* 若没有 buffer，数据将会直接从 Nginx 传输到客户端。假设如果客户端的加载速度足够快，你可以直接把 buffer 关掉，让数据尽可能快地到达客户端。
* 而使用 buffer，Nginx 将会临时存储后端 response 到缓冲区，然后慢慢把数据发送到客户端。启用 buffer 的好处在于可以把数据一次性地发送给目标，相较于即时传输可以节约出这部分带宽。
