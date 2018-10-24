 ===========================================

  **原文链接**：<http://dev.bitshares.works/en/master/development/testnets.html>

 **译者**：https://github.com/towel223 （小办龙的蜗牛）

 **校对者**：

 ===========================================

# 如何设置Nignx网络服务器 #
## 目录 ##
1.安装

2.配置

3.运行Nignx服务

我们在本节用Niginx与Ruby处理并提供访问钱包的文件和水龙头。我们使用的passenger都要手工编译。幸运的是有一个脚本可以完成passenger所有安装。



1.安装

    sudo apt-get install curl libcurl4-gnutls-dev
    cd ~/faucet
    gem install passenger
    sudo passenger-install-nginx-module



在使用推荐设置并等待脚本完成之后，我们将nginx集成到我们的分发通道中。

sudo ln -s /opt/nginx/ /usr/local/nginx
sudo ln -s /opt/nginx/conf/ /etc/nginx



2.配置

Nginx通过刷新安装的nginx.conf文件配置完成的。：


    vi /opt/nginx/conf/nginx.conf

我们如下设置它：


    user gph;
    worker_processes  4;
    
    events {
    worker_connections  1024;
    }
    
    http {
    passenger_root /home/gph/.rbenv/versions/2.2.3/lib/ruby/gems/2.2.0/gems/passenger-5.0.23;
    passenger_ruby /home/gph/.rbenv/versions/2.2.3/bin/ruby;
    passenger_max_request_queue_size 1000;
    
    include   mime.types;
    default_type  application/octet-stream;
    
    access_log  /www/logs/access.log;
    error_log  /www/logs/error.log;
    log_not_found off;
    
    sendfileon;
    #tcp_nopush on;
    
    keepalive_timeout  65;
    
    gzip  on;
    
    upstream websockets {
 
      #把见证人节点的网络的网络套接字的远程端口放在这：
      server localhost:11011;
    }
    
    server {
    listen 80;
    server_name localhost;
    
    location ~ ^/[\w\d\.-]+\.(js|css|dat|png|json)$ {
    root /www/current/public/;
    try_files $uri /wallet$uri =404;
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
    location / {
    passenger_enabled on;
    root /www/current/public/;
    }
    }
    }
    



*注意*

*这两个参数passenger_root和passenger_ruby可能可能会使你的安装不同。然后准备用默认的nginx.conf文件去识别合适的目录。*

我们创建一个_upstream_calledde 网络套接字用于代理http://host/ws的websocket服务器查询。这个操作允许websocket地址与web钱包通用一个端口。


3.运行nginx服务

我们使用nginx服务器脚本并且通过这个脚本完成安装

    sudo wget https://raw.github.com/JasonGiedymin/nginx-init-ubuntu/master/nginx -O /etc/init.d/nginx
    sudo chmod +x /etc/init.d/nginx
    sudo update-rc.d -f nginx defaults
After that, nginx can be launched with:

在此之后，运行nginx：

    sudo service nginx start

*注意*

*重启nginx使用：sudo service nginx reload*
