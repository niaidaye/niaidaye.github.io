## Nginx是什么？

是一个http服务器，Nginx是一款高性能的http服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器

## 能干什么？

- 可以作为Web服务器
- 可以作为邮件服务器
- 可以作为反向代理的服务器
- 动静分离（就是将动态资源和静态资源分隔开）
- 可以实现负载均衡

## 安装步骤：

百度、Google、等等

## 配置文件：

```json
#user  nobody;
#工作的线程(4核8线程那么下面就设置成8 物理硬件有关)
worker_processes  1;

#全局的错误日志存放的地方
#error_log  logs/error.log;

#全局错误日志存放的地方 后面的notice表示的是输出错误日志的格式
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#nginx进程号存放的地方
#pid        logs/nginx.pid;
#最大的连接数(这个也跟硬件是有关系的)
events {
    worker_connections  1024;
}
#下面就是跟HTTP请求有关系的了
http {
    #表示的是当前服务器支持的类型
    include       mime.types;
    #默认传输的数据类型是流 
    default_type  application/octet-stream;

    #声明了一种日志格式 这种日志格式的名字叫做  main
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    #表示的是每一次请求的日志记录 格式就是上面的main格式
    access_log  logs/access.log  main;
    
    #是否可以发送文件
    sendfile        on;
    #tcp_nopush     on;

    #这个是双活的超时的时间
    #keepalive_timeout  0;
    keepalive_timeout  65;

    #是否打开文件的压缩
    #gzip  on;

    #虚拟一个主机
   
   #负载均衡的配置
   upstream qianyu {
      ip_hash;
      server 10.7.182.110:8080;
      server 10.7.182.87:8080;
   }

    #server {
      # listen     90;
      # server_name localhost; 
      # location / {
      #    root qianyu;
      #    index qianyu.html;
      # }
      #做一个反向代理
      #表示的是 如果你访问的后缀是 .jpg结尾的话那么 就访问下面的另外的服务器
       # location ~ \.jpg$ {
       #     proxy_pass   http://10.7.182.110:8080;
       # }

      #下面要演示一个负载均衡的例子
       #location ~ \.jpg$ {
       #    proxy_pass   http://qianyu;
       #}
    #}

   #虚拟一个动静分离的机器
   server {
     listen     9999;
     server_name localhost;
     
     #表示的是动态资源访问的机器

      location / {
            proxy_pass   http://10.7.182.54:8080;
      }
    #表示的是非  css和js文件访问的地址
    location ~ .*\.(htm|html|gif|jpg|jpeg|png|bmp|swf|ioc|rar|zip|txt|flv|mid|doc|ppt|pdf|xls|mp3|wma)$
        {
            root /usr/local/webapp;
            expires 30d;
        }
      #表示的是css和js的访问地址
      location ~ .*\.(js|css)?$
       {
         root /usr/local/webapp;
         expires 1h;
       }

  }

    #虚拟了服务器
    server {
        #端口是  80
        listen       80;
        #这个表示的是虚拟服务器默认访问的是本机
        server_name  localhost;
        #这个是虚拟服务器的编码
        #charset koi8-r;
        #当前服务器的日志的存储的地方
        #access_log  logs/host.access.log  main;

        #做了一个地址的映射
        location / {
            root   html;
            index  index.html index.htm;
        }
        
        #如果报404的时候访问的页面
        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #报错50开头的是 就访问50x.html
        error_page   500 502 503 504  /50x.html;
        #下面对50x.html又做了一个映射  就访问根目录下的html中的  50x.html
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
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}

```

## 反向代理

```json
location ~ \.jpg$ {
            proxy_pass   http://10.7.182.110:8080;
        }
```

## 实现Nginx下的负载均衡

- **默认是轮循的策略：**

```json
配置upstream
   upstream qianyu {
      server 10.7.182.110:8080;
      server 10.7.182.87:8080;
   }
   配置负载均衡
   location ~ \.jpg$ {
           proxy_pass   http://qianyu;
   }
```

- **权重（weight）：**

```json
#负载均衡的配置
  upstream qianyu {
     server 10.7.182.110:8080 weight=2;
     server 10.7.182.87:8080 weight=1;
  }
 配置负载均衡
     location ~ \.jpg$ {
           proxy_pass   http://qianyu;
     }

```

- **IPHash的使用：**

```json
 #负载均衡的配置
   upstream qianyu {
      ip_hash;
      server 10.7.182.110:8080;
      server 10.7.182.87:8080;
   }
配置负载均衡
    location ~ \.jpg$ {
           proxy_pass   http://qianyu;
    }

```

## 动静分离

```json
#虚拟一个动静分离的机器
   server {
     listen     9999;
     server_name localhost;

     #表示的是动态资源访问的机器

      location / {
            proxy_pass   http://10.7.182.54:8080;
      }
    #表示的是非  css和js文件访问的地址
    location ~ .*\.(htm|html|gif|jpg|jpeg|png|bmp|swf|ioc|rar|zip|txt|flv|mid|doc|ppt|pdf|xls|mp3|wma)$
        {
            root /usr/local/webapp;
            expires 30d;
        }
      #表示的是css和js的访问地址
      location ~ .*\.(js|css)?$
  {
         root /usr/local/webapp;
         expires 1h;
       }

  }
```

## 虚拟主机

```json
server {
       listen     90;
       server_name localhost;
       location / {
          root qianyu;
          index qianyu.html;
       }
      #做一个反向代理
      #表示的是 如果你访问的后缀是 .jpg结尾的话那么 就访问下面的另外的服务器
        location ~ \.jpg$ {
            proxy_pass   http://10.7.182.110:8080;
        }
}
```

## 常用命令

1. [ps](http://1.ps/)  aux | grep nginx 查看nginx运行状态
2. systemctl  start  nginx.service  启动nginx
3. nginx -s stop  立即停止nginx服务
4. nginx -s quit  从容停止
5. killall  nginx 杀死所有的nginx 进程
6. systemctl   stop nginx.service  停止nginx服务器
7. systemctl   restart  ngnix.service  重启nginx服务器
8. nginx  -s  reload   重新载入配置文件文件

## 文件位置

1. rpm  -ql  nginx    查看nginx安装列表，可以看到nginx所有的安装位置
2. 总配置文件：/etc/nginx/nginx.conf
3. /etc/nginx/conf.d  所有的nginx自定义配置文件
4. /var/log/nginx/error.log  错误日志文件
5. /usr/share/nginx/html   服务器默认启动目录