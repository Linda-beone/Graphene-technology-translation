
链接：http://dev.bitshares.works/en/master/bts_guide/tutorials/distributed-access-hosting.html
译者：bt666


# 分布式访问比特股去中心化交易所（DEX）

我希望比特股能有更多的接入点和备份WebSocket网关。这是对[运行你自己的去中心化交易所](https://steemit.com/bitshares/@ihashfury/run-your-own-decentralised-exchange)一文的逻辑思考。

[TOC]

## 设置比特股节点

[运行你自己的去中心化交易所](https://steemit.com/bitshares/@ihashfury/run-your-own-decentralised-exchange)

一旦你配置好了全节点，那么比特股的股东就可以通过以下这些步骤安全的访问你的服务器从而进行交易并检查他们的账户。

- 需要DNS别名，用来指向你的服务器IP地址
- 有关DNS别名设置，请参阅[dyn.com](http://dyn.com/)
- 可能需要等待几天才能当DNS运行在网络上
- 将下例中的[altcap.io](http://altcap.io/)改为你的DNS别名

## 创建新用户

我建议在服务器中创建新用户从而运行比特股gui并给用户授予sudo的访问权。你可以使用任何名称，这里，我使用bitshares 作为示例。

    sudo adduser bitshares
    sudo gpasswd -a bitshares sudo
    sudo gpasswd -a bitshares users


## 安装Nginx

安装Nginx网络服务器

    # ssh into your new user bitshares
    ssh bitshares@altcap.io
    sudo apt-get install nginx
    # check version
    nginx -v
    # add user to web server group
    sudo gpasswd -a bitshares www-data
    # start nginx
    sudo service nginx start

这里讲启动默认的Nginx网络服务器。在你的浏览器或别名[altcap.io](http://altcap.io/)上输入ip地址查看。

### 配置Nginx

要配置Web服务器，请编辑默认站点并仅使用http端口80作为新DNS别名，直到你获取letsencrypt.org SSL证书。

### 创建网络文件夹

    sudo mkdir -p /var/www/altcap.io/public_html
    sudo chown -R bitshares:bitshares var/www/altcap.io/public_html
    sudo chmod 755 /var/www

### 配置Nginx

    # edit default setup and save as altcap.io
    sudo nano /etc/nginx/sites-available/default

指向你的虚拟主机
    
    ###### altcap.io ######
    server {
    listen 80;
    server_name altcap.io;
    #rewrite ^ https://$server_name$request_uri? permanent;
    #rewrite ^ https://altcap.io$uri permanent;
    #
    root /var/www/altcap.io/public_html;
    # Add index.php to the list if you are using PHP
    index index.html index.htm;
    #
    location / {
    # First attempt to serve request as file, then
    # as directory, then fall back to displaying a 404.
    try_files $uri $uri/ =404;
    }
    }
    
    CTRL+O to save as altcap.io (^O Write Out)

### 更新虚拟主机文件

    sudo cp altcap.io /etc/nginx/sites-available/altcap.io

### 激活sim连接并禁用默认的网络服务器

    sudo ln -s /etc/nginx/sites-available/altcap.io /etc/nginx/sites-enabled/altcap.io
    sudo rm /etc/nginx/sites-enabled/default

### 将本地文件夹链接到www root并新增简单的index.html文件

    ln -s /var/www/altcap.io/public_html ~/public_html
    nano ~/public_html/index.html

在index.html中添加一些文本信息
    
    <html>
      <head>
    <title>altcap.io</title>
      </head>
      <body>
    <h1>altcap.io - Virtual Host</h1>
      </body>
    </html>
    
    CTRL+X to save as index.html (^X Exit) ###Restart Nginx

    sudo service nginx restart

现在你已经设置好了一个简单的web服务器。DigitalOcean有一篇关于虚拟主机设置的写的非常好的[文章](https://www.digitalocean.com/community/articles/how-to-set-up-nginx-virtual-hosts-server-blocks-on-ubuntu-12-04-lts--3)。
    
## 安装Install letsencrypt

    sudo apt-get install letsencrypt

### 获取SSL证书

    sudo letsencrypt certonly --webroot -w /var/www/altcap.io/public_html -d altcap.io

### 查看证书

    sudo ls -l /etc/letsencrypt/live/altcap.io
    # and check it will update
    sudo letsencrypt renew --dry-run --agree-tos
    sudo letsencrypt renew
    
### 给你的新SSL证书启动新的crojob

    sudo crontab -e

添加下面这行然后每隔6小时，在每个小时的第16分钟运行任务
    
    16 */6 * * *  /usr/bin/letsencrypt renew >> /var/log/letsencrypt-renew.log
    
    CTRL+X to save (^X Exit)
    
    # check your crontab
    sudo crontab -l

### 生成强 Diffe-Hellman组证书

    sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048

### 将SSL添加到Nginx配置中

    cp altcap.io alcap.io.no.ssl

### 编辑  altcap.io

    nano altcap.io

    ###### altcap.io ######
    server {
    listen 80;
    server_name altcap.io;
    #rewrite ^ https://$server_name$request_uri? permanent;
    rewrite ^ https://altcap.io$uri permanent;
    #
    root /var/www/altcap.io/public_html;
    # Add index.php to the list if you are using PHP
    index index.html index.htm;
    #
    location / {
    # First attempt to serve request as file, then
    # as directory, then fall back to displaying a 404.
    try_files $uri $uri/ =404;
    }
    }
    
    
    ###### altcap.io websockets
    
    
    upstream websockets {
    server localhost:8090;
    }
    
    
    ###### altcap.io ssl
    server {
    listen 443 ssl;
    #
    server_name altcap.io;
    #
    root /var/www/altcap.io/public_html;
    # Add index.php to the list if you are using PHP
    index index.html index.htm;
    #
    ssl_certificate /etc/letsencrypt/live/altcap.io/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/altcap.io/privkey.pem;
    #
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    ssl_dhparam /etc/ssl/certs/dhparam.pem;
    ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA';
    ssl_session_timeout 1d;
    ssl_session_cache shared:SSL:50m;
    ssl_stapling on;
    ssl_stapling_verify on;
    add_header Strict-Transport-Security max-age=15768000;
    #
    # Note: You should disable gzip for SSL traffic.
    # See: https://bugs.debian.org/773332
    #
    # Read up on ssl_ciphers to ensure a secure configuration.
    # See: https://bugs.debian.org/765782
    #
    # Self signed certs generated by the ssl-cert package
    # Don't use them in a production server!
    #
    # include snippets/snakeoil.conf;
    #
    location / {
    # First attempt to serve request as file, then
    # as directory, then fall back to displaying a 404.
    try_files $uri $uri/ =404;
    }
    location ~ /ws/? {
    access_log off;
    proxy_pass http://websockets;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    }
    }
    ###### altcap.io ######
    
    CTRL+X to save (^X Exit)
    You have now setup an SSL secured web server with a WebSocket connected to your local BitShares witness_node (listening on port 8090 - see this post for more information) ###Update altcap.io www virtual host
    
    sudo cp altcap.io /etc/nginx/sites-available/altcap.io

### 重启Nginx

sudo service nginx restart

现在你已经配置好了SSL web服务器。更多关于SSL设置的信息可以查看[ DigitalOcean letsencrypt SSL LetsEncrypt CertBot](DigitalOcean letsencrypt SSL LetsEncrypt CertBot)

## 安装比特股网页GUI

## 安装NVM(节点版本管理)

    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.30.2/install.sh | bash

退出终端并重连

    ssh bitshares@altcap.io
    nvm install v5
    nvm use v5

### 下载比特股GUI

- [https://github.com/bitshares/bitshares-ui/releases](https://github.com/bitshares/bitshares-ui/releases)

### 设置轻钱包

<table style="width: 750px;"><tbody>
    <tr>
        <th bgcolor="LightBlue">注意</th>
    </tr>
    <tr>
        <td bgcolor="MintCream"> 请查阅bitshares-ui 安装指南</td>
    </tr>
</table>

### 构建轻钱包

    npm run build

现在你已经给比特股去中心化交易所添加了另一个接入点。请查看一下防火墙和SSH是否是最新版的，而且配置是否正确。DigitOcean有[防火墙](https://www.digitalocean.com/community/tags/firewall?type=tutorials)和[安全SSH](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2)教程，可以查阅。


### SSL测试

你也可以测试一下相较于银行而言，你的新的web服务器有多安全。在浏览器中输入下面的链接并等待结果

    https://www.ssllabs.com/ssltest/analyze.html?d=altcap.io

将链接中的altcap.io改为你本地银行的域名并贴出结果。


