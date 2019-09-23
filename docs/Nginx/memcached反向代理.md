# memcached反向代理

> ## memcached教程

?> Memcached是一个自由开源的，高性能，分布式内存对象缓存系统(Nosql)。Memcached简洁而强大。它的简洁设计便于快速开发，减轻开发难度，解决了大数据量缓存的很多问题。它的API兼容大部分流行的开发语言。本质上，它是一个简洁的key-value存储系统。

### 目的 {docsify-ignore}

一般的使用目的是，通过缓存数据库查询结果，减少数据库访问次数，以提高动态Web应用的速度、提高可扩展性。

![memcached](http://www.runoob.com/wp-content/uploads/2015/08/web_6.jpg)

### Linux Memcached

#### Memcached 安装

`yum install memcached`

#### Memcached 运行

`$ memcached -u root -p 11111 -m 64m -vv`

启动选项：
- -d是启动一个守护进程；
- -m是分配给Memcache使用的内存数量，单位是MB；
- -u是运行Memcache的用户；
- -l是监听的服务器IP地址，可以有多个地址；
- -p是设置Memcache监听的端口，，最好是1024以上的端口；
- -c是最大运行的并发连接数，默认是1024；
- -P是设置保存Memcache的pid文件。

#### 使用Memcached

##### 连接 Memcached

`$ telnet HOST PORT`

##### 存储 Memcached

`set key flags exptime bytes [noreply] `  
`value`

!> 注意bytes后面需要有空格，否则报错，无法输入value。

- key：键值 key-value 结构中的 key，用于查找缓存值。
- flags：可以包括键值对的整型参数，客户机使用它存储关于键值对的额外信息 。
- exptime：在缓存中保存键值对的时间长度（以秒为单位，0 表示永远）
- bytes：在缓存中存储的字节数
- noreply（可选）： 该参数告知服务器不需要返回数据
- value：存储的值（始终位于第二行）（可直接理解为key-value结构中的value）

查找 Memcached

`get key1 key2 key3`

> ## Nginx反向代理Memcached

### 功能  {docsify-ignore}

- 将HTTP请求转换为memcached协议中的get请求，转发请求至上游memcached服务
- 通过$memcached_key构造key键

### 模块  {docsify-ignore}

- ngx_http_memcached_module(默认编译)
- --without-http_memcached_module

### memcached指令

|                      | memcached反向代理               | http反向代理                |
| :------------------- | :------------------------------ | :-------------------------- |
| 指定上游             | memcached_pass                  | proxy_pass                  |
| 连接绑定地址         | memcached_bind                  | proxy_bind                  |
| 接收响应头部缓存     | memcached_buffer_size           | proxy_buffer_size           |
| 连接上游超时时间     | memcached_connect_timeout       | proxy_connect_timeout       |
| 强制使用range协议    | memcached_force_ranges          | proxy_force_ranges          |
| 针对key-flag使用gzip | memcached_gzip_flag             | -                           |
| 出错时更换上游       | memcached_next_upstream         | proxy_next_upstream         |
| 更换上游超时         | memcached_next_upstream_timeout | proxy_next_upstream_timeout |
| 更换上游重试次数     | memcached_next_upstream_tries   | proxy_next_upstream_tries   |
| 读取响应超时时间     | memcached_read_timeout          | proxy_read_timeout          |
| 发送请求超时时间     | memcached_send_timeout          | proxy_send_timeout          |
| 使用TCPkeepalive     | memcached_socket_keepalive      | proxy_socket_keepalive      |

> ## 实例：Nginx反向代理Memcached

### 配置 Memcached  {docsify-ignore}

#### 1.启动 memcached

`$ memcached -u root -p 11111 -m 64m -vv`  

#### 2.连接 memcached

`$ telnet 127.0.0.1 11111`

#### 3.设置key

`$ set hello 2 0 5 `  
`world`

输出

    STORED

#### 4.查询key

`$ get hello`

输出

    VALUE hello 2 5
    world
    END

### Nginx.conf

```nginx
server {
    listen 8115;
    server_name localhost;
    
    location / {
        #设置请求memcached的key
        set $memcached_key $arg_key;
        #当响应key的flag为2时，开启gzip
        memcached_gzip_flag 2;
        #代理memcached地址
        memcached_pass 127.0.0.1:11111;
    }
}
```
### 测试  {docsify-ignore}

#### 1.测试key

`$ curl 192.168.18.129:8115/get?key=hello`

输出

    world

#### 2.测试是否开启gzip

`$ curl 192.168.18.129:8115/get?key=hello -I`

输出

    Content-Encoding: gzip