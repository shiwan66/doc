### 安装 Nginx

在 CentOS 上，可直接使用 `yum` 来安装 Nginx

```
yum install nginx -y

```

安装完成后，使用 `nginx` 命令启动 Nginx：

```
nginx

```

此时，访问 [http://<您的域名>](http://<您的域名 "null") 可以看到 Nginx 的测试页面 <sup>[[?](#stage-4-step-1-help)]</sup>

<a id="stage-4-step-1-help"></a>

> 如果无法访问，请重试用 `nginx -s reload` 命令重启 Nginx

### 配置 HTTPS 反向代理

外网用户访问服务器的 Web 服务由 Nginx 提供，Nginx 需要配置反向代理才能使得 Web 服务转发到本地的 Node 服务。

先将之前下载的 SSL 证书(解压后 Nginx 目录分别以 crt 和 key 作为后缀的文件)通过`拖动到左侧文件浏览器/etc/nginx目录`的方式来上传文件到服务器上

_如何上传 SSL 证书到 /etc/nginx 目录_

Nginx 配置目录在 _/etc/nginx/conf.d_，我们在该目录创建 _ssl.conf_

##### 示例代码：/etc/nginx/conf.d/ssl.conf

```
server {
        listen 443;
        server_name t.shiwan66.top; # 改为绑定证书的域名
        # ssl 配置
        ssl on;
        ssl_certificate 1_t.shiwan66.top_bundle.crt; # 改为自己申请得到的 crt 文件的名称
        ssl_certificate_key 2_t.shiwan66.top.key; # 改为自己申请得到的 key 文件的名称
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
        ssl_prefer_server_ciphers on;

        location / {
            proxy_pass http://127.0.0.1:8765;
        }
    }

```

按 `Ctrl + S` 保存配置文件，让 Nginx 重新加载配置使其生效：

```
nginx -s reload

```


## WebSocket 配置

```
# WebSocket 配置
map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      close;
}
server {
        listen 443;
        server_name t.shiwan66.top; # 改为绑定证书的域名
        # ssl 配置
        ssl on;
        ssl_certificate 1_t.shiwan66.top_bundle.crt; # 改为自己申请得到的 crt 文件的名称
        ssl_certificate_key 2_t.shiwan66.top.key; # 改为自己申请得到的 key 文件的名称
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
        ssl_prefer_server_ciphers on;

        # WebSocket 配置
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;

        location / {
            proxy_pass http://127.0.0.1:8765;
        }
    }

```