 **原文链接**：[http://dev.bitshares.works/en/master/bts_guide/apis-faq.html#api-core-faq-1](http://dev.bitshares.works/en/master/bts_guide/apis-faq.html#api-core-faq-1)
 
**译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
**校对者**： 
 
---
# API - 常见问答

## 如何使用network_add_nodes命令？为什么这么复杂？

你需要按照“访问受限制的API”部分中的说明进行操作，允许使用用户名/密码访问network_node_ API。然后你要用命令行或者配置文件将用户名/密码传递给cli _wallet。

它以这种方式设置，即使RPC端口可公开访问，默认配置也是安全的。如果你的witness_node允许一般公众查询数据库或广播交易（事实上，这就是托管的WebUI的工作方式），那就没问题。如果你的witness\_node允许一般公众控制连接的P2P节点，那就不太好。因此API添加p2p连接需要设置适当的访问控制。  

## 有没有一种需要通过HTTP登录的访问方法？

没有，登录本质上是一个有状态的过程（登录更改了什么，服务器将对某些请求进行处理，这点是有意义的）。如果需要跨HTTPRPC调用跟踪状态，则必须维护跨多个连接的会话。这是一个著名的来源HTTP应用程序的安全漏洞。另外，HTTP并非真正设计用于“服务器推送”通知，我们会找出一种为轮询客户端排队通知的方法。

Websockets解决了所有这些问题。如果你需要访问石墨烯的状态方法，你需要使用WebSockets。
 
## 有没有办法允许外部程序通过websocket，JSONRPC或HTTP驱动cli _wallet？

有得。外部程序可以连接到CLI钱包通过websocketsAPI发起调用。为此，请在服务器模式下运行钱包，即cli_wallet-s“127.0.0.1:9999”然后有外部程序通过指定端口连接到它（在本例中为端口为9999）。
 

## 有没有一种生成参数名称和方法描述的帮助的办法？

有的. 可以生成代码库的文档，包括APIs,使用Doxygen生成的。只需在此目录中运行doxygen。

如果你的构建环境中都有Doxygen和perl，那么CLIwallet的help和gethelp命令将显示生成的doxygen帮助文档。

如果你的CLI钱包的帮助命令显示说明没有参数名称如signed_transaction transfer（string，string，string，string，string，bool）这意味着在配置过程中，CMake无法找到Doxygen或perl。如果找到，输出应该如下：
this: signed\_transaction transfer(string from, string to, string amount, string asset\_symbol, string memo, bool broadcast)
 

# a.b.c数字是什么意思？

第一个数字指定空间。空间1用于协议对象，2用于实现对象。协议空间对象可以出现在
例如，以二进制形式的交易。履行空间对象只能存在于实现目的，例如优化或内部记账。

第二个数字指定类型。对象的类型决定它有哪些域。有关类型ID的完整列表，请参阅枚举
types.hpp中的object_type和impl_object_type。

第三个数字是指定实例。对象的实例对每个个体对象是不同的。

# 上一个问题的答案令人困惑。你能说得更清楚吗？

所有帐户ID的格式均为1.2.x.如果你是要注册后的第9735个帐户，你的帐户ID将是1.2.9735。帐户0是特别的（这是“委员会帐户”，由委员会成员和其他一些有能力和限制的帐户控制）。

所有资产ID的格式均为1.3.x.如果你是注册后的第29个资产，你的资产ID将为1.3.29。资产0是特殊的（它是BTS，被认为是“核心资产”）。

第一个和第二个数字一起确定了你说的那种事物（账户1.2，资产1.3）。第三个数字
识别特定的事物。