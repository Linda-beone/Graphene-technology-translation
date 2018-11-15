原文链接：https://steemit.com/witness-category/@full-steem-ahead/howto-using-nginx-to-load-balance-servers-part-2

译者：bt666

# 如何:使用Nginx负载均衡服务器 第2部分 #

在我之前的文章中，我描述了如何向websocket服务见证人节点中添加负载均衡代理，并承诺会写一篇有关如何添加LetsEncrypt SSL证书的后续文章，因此本文就是后续文章。

添加免费的LetsEncrypt SSL证书到服务器上很容易，没有重复的开销并且可以防止中间人攻击。 LetsEncrypt使用ACME（自动证书管理环境）协议，将证书与DNS域和网站绑在一起，因此过期的证书可能已成为过去。我想不出有什么好的理由不为所有网站使用SSL。

前一篇文章假设您仅使用您的Nginx服务器代理或负载均衡您的见证人websocket服务器，但本文描述了一种更佳的配置，其中包括一些虚拟服务器文件，可以支持受SSL保护的websocket代理，负载均衡，安全bts_tools网站托管以及极限峰值响应，用来处理非SSL端口80上的LetsEncrypt SSL证书的更新。

以下是[我的github](https://github.com/ThomasFreedman/nginx-proxy.git)仓库的相关链接，上一篇文章和[@jesta](https://steemit.com/@jesta)的[原文](https://steemit.com/witness-category/@full-steem-ahead/howto-using-nginx-to-load-balance-servers)。您还需要下载并安装[getssl bash脚本](https://github.com/srvrco/getssl)，该脚本实现证书的acme-challenge协议。作为bash脚本，它几乎没有什么要求，但确实需要curl和nslookup。如果还没安装nslookup的话，Debian或Ubuntu系统中的dnsutils包中提供了nslookup。

关于使用getssl脚本的完整说明可以在我上面链接的github仓库上找到。要设置三项：
1）配置文件（2），脚本描述要获取证书的域
2）极限峰值所需的Nginix配置
3）运行脚本以更新证书的Crontab项

证书管理设置主要与Nginx配置相独立且自治。在我的github仓库中的两个ssl_server *脚本中已经提供了/.well-known/acme-challenge的定义。我建议首先安装Nginx，以便它可以从/.well-known/acme-challenge/文件夹中提供文件，然后安装getssl脚本以获取SSL证书。如果要运行受SSL保护的bts_tools接口以及代理的websocket见证人节点，则需要为其创建单独的域名，以便服务器可以区分将请求路由到哪个虚拟服务器（websocket或bts_tools），因为它们都使用标准SSL端口433.如果要在不同端口上运行它们，你可以不用使用其他域名。这取决于你的选择。

** ssl_server_lb.conf **文件用于定义负载均衡服务器，该服务器将请求分派到SSL websocket服务器池（wss：// ...）。它还提供独立的，非负载均衡的虚拟服务器，从而代理本地websocket服务器，bts_tools和端口80上的极限峰值服务器。

如果在负载均衡服务器上运行见证人节点，而该服务器又属于待负载均衡的服务器池中，本地服务器需要一个虚拟服务器定义，用来响应与非SSL ws见证人服务器不同端口的请求，并且负载均衡服务器使用标准的SSL 433端口。原因是负载均衡器只均衡SSL请求，并将它们分派到SSL websocket服务器。本地witness_node无法直接处理SSL加密，它需要Nginx进行解密并将未加密的ws请求传递给它。任何未使用的本地端口都可以在1024以上使用。

除此之外没有什么了。不要忘记更新/etc/nginx/nginx.conf文件中用于负载均衡的那一行，并为共享内存定义ws区。

再次感谢[@jesta](https://steemit.com/@jesta)和[@reolandp](https://steemit.com/@reolandp)的工作和建议。一如既往地感谢您的支持！

图片用于赞扬github用户[lukas2511](https://github.com/lukas2511/dehydrated)，感谢他的替代性的bash脚本以及有趣的logo。
