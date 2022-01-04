## Nginx

### 一、Nginx应用

1. Nginx可以作为 Web 应用服务器

   Ngxin可以作为静态页面的服务器，同时还支持CGI协议的动态语言，如perl，php等。Nginx为性能优化而研发，能够支持5000个并发连接数

2. 正向代理

   **如果把局域网外的 Internet 想象成一个巨大的资源库，则局域网中的客户端要访 问 Internet，则需要通过代理服务器来访问，这种代理服务就称为正向代理**

   

   ![正向代理](D:\Workspace\ImageStore\Nginx\正向代理.png)

3. 反向代理

   - 反向代理，**其实客户端对代理是无感知的，因为客户端不需要任何配置就可以访问**。

   - **我们只 需要将请求发送到反向代理服务器，由反向代理服务器去选择目标服务器获取数据后，在返 回给客户端**，此时反向代理服务器和目标服务器对外就是一个服务器，**暴露的是代理服务器 地址，隐藏了真实服务器 IP 地址。**

   ![反向代理](D:\Workspace\ImageStore\Nginx\反向代理.png)

4. 负载均衡

   - 增加服务器的数量，然后将请求分发到各个服务器上，将原先请求集中到单个服务器上的 情况改为将请求分发到多个服务器上，将负载分发到不同的服务器，也就是我们所说的负 载均衡
   - 客户端发送多个请求到服务器，服务器处理请求，有一些可能要与数据库进行交互，服 务器处理完毕后，再将结果返回给客户端。

   负载均衡主要作用

   >**高并发**：负载均衡通过算法调整负载，尽力均匀的分配应用集群中各节点的工作量，以此提高应用集群的并发处理能力（吞吐量）。
   >
   >**伸缩性**：添加或减少服务器数量，然后由负载均衡进行分发控制。这使得应用集群具备伸缩性。
   >
   >**高可用**：负载均衡器可以监控候选服务器，当服务器不可用时，自动跳过，将请求分发给可用的服务器。这使得应用集群具备高可用的特性。
   >
   >**安全防护**：有些负载均衡软件或硬件提供了安全性功能，如：黑白名单处理、防火墙，防 DDos 攻击等。

5. 动静分离

   为了提高网站的解析速度，可以把动态页面和静态页面分开进行解析、

   

   ![动静分离](D:\Workspace\ImageStore\Nginx\动静分离.png)

### 二、Ngxin操作

#### 2.1 下载与安装（centOS）

- 使用解压缩包的方式

1. **安装pcre**
   上传源码压缩包，解压、编译、安装 三部曲。
   1）、解压文件， 进入pcre目录，
   2）、./configure 完成后，
   3）、执行命令： make && make install
2. **安装 openssl**
   下载OpenSSL的地址:
   [http://distfiles.macports.org/openssl/](https://link.zhihu.com/?target=http%3A//distfiles.macports.org/openssl/)
   1）、解压文件， 回到 openssl目录下，
   2）、./configure 完成后，
   3）、执行命令： make && make install
3. **安装 zlib**
   1）、解压文件， 回到 zlib 目录下，
   2）、./configure 完成后，
   3）、执行命令： make && make install
4. **安装 nginx **
   1）、解压文件， 回到 nginx 目录下，
   2）、./configure 完成后，
   3）、执行命令： make && make install

#### 2.2 运行

安装完成 Nginx 后，会在 `/usr/local/` 自动生成 nginx 文件夹

**版本查看**

```shell
./nginx -v
```

**启动**

```shell
./nginx

# 查看是否启动
ps -ef | grep nginx
```

**停止**

```shell
./ngxin -s stop
```

**重新加载**

```shell
./nginx -s reload
```

#### 2.3 配置文件

文件路径 `/usr/local/nginx/conf/nginx.conf`

``` configuration
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
	server_name  www.crazyqiqi.top;

        location / {
            root   /home/hexo/public;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
        
        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}


    }

    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    server {
        listen       443 ssl;
        server_name  www.crazyqiqi.top;

        ssl_certificate      /usr/local/nginx/ssl/5500590_www.crazyqiqi.top.pem;
        ssl_certificate_key  /usr/local/nginx/ssl/5500590_www.crazyqiqi.top.key;

        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;

        location / {
            root   /home/hexo/public;
            index  index.html index.htm;
        }
    }
	
}

```

**配置文件说明**

1. 全局块

   主要包括配 置运行 Nginx 服务器的用户（组）、允许生成的 worker process 数，进程 PID 存放路径、日志存放路径和类型以 及配置文件的引入等。

   ```conf
   # 允许生成 worker process 数
   worker_processes  1;
   
   # 日志存放路径和类型
   #error_log  logs/error.log;
   #error_log  logs/error.log  notice;
   #error_log  logs/error.log  info;
   
   # 进程PID
   #pid        logs/nginx.pid;
   ```

   

2. events块

   events 块涉及的指令**主要影响 Nginx 服务器与用户的网络连接，常用的设置包括是否开启对多 work process 下的网络连接进行序列化，是否 允许同时接收多个网络连接，选取哪种事件驱动模型来处理连接请求，每个 word process 可以同时支持的最大连接数等。**

   ```conf
   events {
       worker_connections  1024;
   }
   ```

   上述例子就表示每个 work process 支持的最大连接数为 1024.

3. http和server块

   ```conf
   http {
       include       mime.types;
       default_type  application/octet-stream;
   
       sendfile        on;
   
       #keepalive_timeout  0;
       keepalive_timeout  65;
   
       #gzip  on;
   
   	# 监听端口
       server {
           listen       80;
   		server_name  www.crazyqiqi.top;
   
           location / {
           	# 文件路径
               root   /home/hexo/public;
               # index文件
               index  index.html index.htm;
           }
   
           #error_page  404              /404.html;
   
           # redirect server error pages to the static page /50x.html
           #
           error_page   500 502 503 504  /50x.html;
           location = /50x.html {
               root   html;
           }
   
       }
       
       # 反向代理模块
       # proxy the PHP scripts to Apache listening on 127.0.0.1:80
       #
       #location ~ \.php$ {
       #    proxy_pass   http://127.0.0.1;
       #}
   
       # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
       #
       #location ~ \.php$ {
       #    root           html;
       #    fastcgi_pass   127.0.0.1:9000;
       #    fastcgi_index  index.php;
       #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
       #    include        fastcgi_params;
       #}
   
       # deny access to .htaccess files, if Apache's document root
       # concurs with nginx's one
       #
       #location ~ /\.ht {
       #    deny  all;
       #}
   
       # 反向代理模块
       # HTTPS server
       #
       server {
           listen       443 ssl;
           server_name  Domain Name;
   
           ssl_certificate      /usr/local/nginx/ssl/Domain_Name.pem;
           ssl_certificate_key  /usr/local/nginx/ssl/Domain_Name.key;
   
           ssl_session_cache    shared:SSL:1m;
           ssl_session_timeout  5m;
   
           ssl_ciphers  HIGH:!aNULL:!MD5;
           ssl_prefer_server_ciphers  on;
   
           location / {
           	# 文件路径
               root   /home/hexo/public;
               # index 文件
               index  index.html index.htm;
           }
       }
   	
   }
   ```

   

### 参考文章

[nginx学习，看这一篇就够了!](https://zhuanlan.zhihu.com/p/362315737)