# slice模块分片提升缓存效率

### slice指令

```nginx
Syntax: slice size;
Default: slice 0;
Context: http, server, location
```

`设置切片的大小。`

#### 功能

通过range协议将大文件分解为多个小文件，更好的用缓存为客户端的range协议服务。

#### 模块

- http_slice_module
- --with-http_slice_module

#### 运行流程

![运行流程](https://raw.githubusercontent.com/shangyewangchuan/material/master/img/nginx_slice_1.png)

> ## 实例 ：利用slice模块提升缓存效率

### Nginx.conf

```nginx
server {
    listen 8116;
    server_name localhost;

    location / {
        #设置分片大小
        slice 1m;
        #设置key
        proxy_cache_key $uri$is_args$args$slice_range;
        #设置代理Range头部
        proxy_set_header Range $slice_range;
        proxy_cache_valid 200 206 1m;
        add_header X-Cache-Status $upstream_cache_status;
        proxy_pass http://localhost:8117; 
    }
}
server {
    listen 8117;
    server_name localhost;

    location / {
    }
}
```

### 测试

#### 1.测试不开启slice

`curl -r 40000000-40001000 192.168.18.129:8116/xman -I`

- 终端结果

```
Content-Length: 1001
Content-Range: bytes 40000000-40001000/49656648
```

- access.log :

```
127.0.0.1 - - [14/Apr/2019:21:29:45 +0800] "GET /xman HTTP/1.0" 200 49656648 "-" "curl/7.29.0"
192.168.18.129 - - [14/Apr/2019:21:29:56 +0800] "GET /xman HTTP/1.1" 200 8046745 "-" "curl/7.29.0"
```

#### 2.测试开启slice

`curl -r 40000000-40001000 192.168.18.129:8116/xman -I`

- 终端结果

```
Content-Length: 1001
Content-Range: bytes 40000000-40001000/49656648
```

- access.log :

```
127.0.0.1 - - [14/Apr/2019:21:55:15 +0800] "HEAD /xman HTTP/1.0" 206 0 "-" "curl/7.29.0"
192.168.18.129 - - [14/Apr/2019:21:55:15 +0800] "HEAD /xman HTTP/1.1" 206 0 "-" "curl/7.29.0"
```