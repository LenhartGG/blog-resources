# Windows环境下 vue @cli3 项目使用 niginx 部署
本项目采用前后端分离框架vue @cli3，独立于后端部署前端应用，也就是说后端暴露一个前端可访问的API，然后前端实际上是纯净态应用。那么可以将 `dist` 目录里构建的内容部署到任何静态文件服务器中，但要确保正确的 `publicPath`.

## 使用 history 的路由的问题
如果是在 history 模式下使用 Vue Router，是无法搭配简单的静态文件服务器的。例如，如果你使用 Vue Router 为 /todos/42/ 定义了一个路由，开发服务器已经配置了相应的 localhost:3000/todos/42 响应，但是一个为生产环境构建架设的简单的静态服务器会却会返回 404。

为了解决这个问题，你需要配置生产环境服务器，将任何没有匹配到静态文件的请求回退到 `index.html` 。

## HTML5 History 模式
`vue-router` 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。例如：`http://127.0.0.1:999/#/login`


但是我们不想要很丑的 hash，我们可以用路由的 **history 模式**，这种模式利用 history.pushState API 来完成 URL 跳转而无须重新加载页面。
```javascript
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```
当你使用 history 模式时，URL 就像正常的 url，例如 `http://yoursite.com/user/id`，看起来比较顺眼。

但是这种模式需要后台的配置支持。因为我们的应用是个单页客户端应用，如果后台没有正确的配置，当用户在浏览器直接访问 `http://oursite.com/user/id` 就会返回 404

## 后端配置例子

### nginx
关于windows下的nginx的安装这里就不在赘述，点击链接直接去官网下载即可：http://nginx.org/en/download.html
```javascript
location / {
  try_files $uri $uri/ /index.html;
}
```

## 警告
需要注意的是，如果这么做的话，服务器就不再返回 404 错误页面，因为对于所有路径都会返回 `index.html` 文件。为了避免这种情况，我们需要在 Vue 应用里面覆盖所有的路由情况，然后在给出一个 404 页面。

## 如何部署nginx
1.将 `npm run build` 打包后的`dist`文件夹复制到nginx的html文件夹下面；
2.打开nginx.conf配置文件：
```config

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

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       9999; #服务器的端口号
        server_name  localhost:127.0.0.1; #IP或者域名，localhost：127.0.0.1 本地的IP

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        root   D:/Programs/WorkPlace/Engineer-Headline-Desktop/headline_frontend/dist;
        
    　  #解决history路由的问题
        location / {
            try_files $uri $uri/ /index.html;
        }


        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   D:/Programs/WorkPlace/Engineer-Headline-Desktop/headline_frontend/dist;
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

3.在nginx文件中，按下 `shift+右键` 点击 `command` 打开命令行，输入 `nginx start` 启动服务，在页面中输入localhost:9999或者本机ip即可查看页面。

关于 nginx 的常用命令，我在这里列举一下：

- 1.重启服务： service nginx restart 

- 2.快速停止或关闭Nginx：nginx -s stop

- 3.正常停止或关闭Nginx：nginx -s quit

- 4.配置文件修改重装载命令：nginx -s reload