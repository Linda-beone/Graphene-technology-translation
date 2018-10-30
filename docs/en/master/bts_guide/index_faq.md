

常见问题 (FAQ) 
********************************

概要
------------------------------

* 标准的Bitshares地址结构和格式是什么样的?
* 块头的格式是什么? 
* 最大bitshares区块大小是多少? - 可通过链参数配置
* 目前是否部署了任何分片机制？
* SPV客户端如处理的？
* 在区块链中如何解决时间问题？是使用NTP还是其他一些协议？
* 平均区块时间是多少？
* 如何在Bitshares解决会计问题？它是Nxt风格的会计模型还是比特币的UTXO
* 如何验证交易？
* Bitshares交易的平均字节大小是多少？
* 本地支持哪些交易类型？
* 数据目录中的配置文件中有那些哪些参数？ -（rel：config.ini）？  
* 是否有Dockerfile支持? 

**协议**

* BitShares有哪些类型的协议? 
* 是否有任何特殊的隐私保护？
* 协议是否为覆盖协议提供了交互机制，例如OR_RETURN？
* 这是通过谣传的协议还是通过中继完成的？

**数据结构**

* 区块链中使用了哪些数据结构？
* 如何构造每个块以及创建区块的元素类型是什么? 
* BitShares有哪些类型的对象和元素? 

**公钥系统**

* 使用什么样的公钥系统？如果是椭圆曲线，那么曲线是什么？

**脚本语言**

* 脚本语言是否完整？
* 是否有Bitshares脚本语言的规范？ （假设有一个）

-------------------------------------



系统组件
-------------------------------------------------------

* 区块如何构建？
* 我在哪里可以找到协议信息? 
* BitShares-Core提供哪些类型的操作? 
* BitShares-Core中有哪些类型的对象和元素? 
* 配置示例和支持的文件

  - config.ini  (节点配置文件 )
  - apiConfig.js (BitShares 公共节点信息)
  - api-access.json (API 访问限制)
  - saltpass.py(从密码中获取哈希值和salt值)
  
账户
-------------------------------------------------------

* 我在哪里可以阅读BitShares帐户信息?
  - 包括：会员资格，费用，权限，公钥和私钥，多重签名，投票，推荐计划和归属余额

  
安装选项
--------------------------------------------------------

* 安装BitShares-Core可以使用哪种开发环境？
  
  * Ubuntu Linux
  * OS X
  * Windows
  * Windows - CLI Tools
  * Windows Subsystem for Linux
  
* 系统要求是什么样的?



APIs
---------------------------------------------------------
* API限制是什么样的？如何使用？ (rel:*api-access.json*)
* 如何通过HTTP访问API
* 如何使用远程过程调用（RPC）
* 如何使用Websocket调用
* 哪里可以找到doxygen文档
* 如何定义BitShares对象和IDs
* 如何使用network_add_nodes命令？为什么这么复杂？
* 有没有一种需要通过HTTP登录的访问方法？
* 有没有办法允许外部程序通过websocket，JSONRPC或HTTP驱动cli _wallet？
* 有没有一种生成参数名称和方法描述的帮助的办法？
* a.b.c数字是什么意思？
* 上一个问题的答案令人困惑。你能说得更清楚吗？
* 我在哪里可以找到一个简单的Python脚本saltpass.py来从密码中获取哈希值和salt值? (rel: *saltpass.py*)
* 我在哪里可以找到BitShares公共全节点信息？（apiconfig）


**BitShares 浏览器 API**

* 我可以试试BitShares 浏览器 API吗？
* 如何安装BitShares 浏览器 REST API  - 安装指南
 

资产
-------------------------------------------------------

* 资产创建费会怎样？
* 创建资产后，我可以更改吗？
* 父资产和子资产是怎么样的？
* 我可以更改发行人吗？

*矿池费用*

* 什么是费用池排水？
* 什么是费用池？
* 如果费用池是空的，该怎么办？

*市场费用*

* 如何申请累积费用？
* 资产标志和权限是什么？
* 标志是什么？
* 权限是什么？
* 如果我启用市场费用会怎样？
* 如果交易涉及两种不同的市场费用怎么办？

*市场锚定资产*

* 我可以使用与UIA相同的标志/权限吗？
* 什么是市场锚定资产特定参数？


转移/交易
-------------------------------------------------------
* 锁链交易（维基）


CLI 钱包问题
-------------------------------------------------------
* 如何以完整的方式关闭CLI客户端？
* 当我第一次尝试运行CLI客户端时，为什么会立即崩溃？
* wallet.json有哪些参数? - (rel:*wallet.json*)

见证人问题
-------------------------------------------------------
* 如何以的完整方式关闭见证人节点？  
* 如何检查见证人节点是否已同步？
* 如果它似乎超过某个日期无法同步
* 删除存储的在witness\_node\_data\_dirlogsp2p下的日志是否安全？
* 与见证人节点交互的最佳方式是什么？
* 公共和私有testnet有什么区别？
* 见证人节点控制台中所有这些不同文本颜色的含义是什么？
* 谁的私钥是“[BTS6MRyAjQ ..”，“5KQwrPbwdL ..”]？为什么在config.ini中预定义了？


---

# 资产 - 常见问答

## 创建资产后，我可以更改吗？

创建后可以更改以下参数:

  - 发行者

  -    UIA-选项:
        
          - 最大供给
          - 市场费用
          - 权限（仅禁用/不重新启用）
          - 标志（如果权限允许）
          - 核心汇率
          - 黑/白 名单
          - 描述

  -    MPG-选项:
        
          - 喂价生命周期
          - 最小喂价
          - 强制清偿 偏移/延时/数量

不能改变的参数:

  - 符号
  - 精确度

可以在这里找到指南。

## 我可以更改发行人吗？

当前的资产发行可能会将资产的所有权转移给其他人通过更改资产设置中的发行人。

## 父资产和子资产是怎么样的？

资产的**父** / **子**关系可以表示为符号的名称，例如：

    PARENT.child

只能由“父”发行者创建，而不能由其他人创建。

## 资产创建费会怎样？

50％的资产创建费用于预先填充资产费用池。另外50％中，20％进入网络，80％进入推荐程序。这意味着，如果你是终身会员，你就会得到归属期后的资产创建费的40％（目前为90天）。


### 矿池费用

## 什么是费用池排水？

如果在非BTS资产中创建并支付订单，则费用为隐含地转换为BTS以支付网络费用。但是，如果订单被取消，90％的费用将作为BTS退还。结果是，如果核心汇率低于最高出价，人们可以简单地从市场上购买你的代币，并隐含地交换它们利用矿池费用通过创建和取消订单。这将耗尽费用池并带给发行人代币轻微损失（取决于核心汇率的补偿）。出于这个原因，我们建议使用略高于你的资产市场价的核心汇率。因此，使始终支付BTS的费用更便宜。


## 什么是费用池？

费用池允许网络中的参与者无需持有BTS即可处理资产和支付交易费用。任何交易费可以通过支付任何核心汇率（即价格）的资产隐含转换成BTS支付网络费用。如果资产的费用池有资金的，那么费用可以在本地UIA而不是比特股支付。 
 <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="lightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream"> 核心汇率可以将费用兑换成BTS，可能与资产的实际市场价值不同。因此，用户可以通过用BTS支付来支付保费或备用金。</td>
    </tr>
    </table>

<table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="sandybrown">警告</td>
    </tr>
    <tr>
        <td bgcolor="MintCream"> 确保你的核心汇率高于最低要求，否则，人们会从市场上通过隐含的套利购买你的代币并消耗你的费用池。</td>
    </tr>
    </table>

发行人的任务是保持费用池的资金和核心汇率更新，除非他想要他的资产所有者需要持有BTS作为费用。 

打开发行人的帐户，单击资产选项卡并打开用于更改资产的对话框。将有一个费用池选项卡，允许你为费用池提供资金并领取累计费用！

-----

## 市场费用

打开发行人的帐户，单击资产选项卡并打开用于更改资产的对话框。将有一个费用池选项卡，允许你为费用池提供资金并领取累计费用！

创建资产时，发行人可以设置任何组合标志/权限。**标志**是固定的，除非有**权限**允许更改。一旦撤消编辑权限，就会标记为永久性的，永远不能再修改。

  - `charge_market_fee`: 发行人指定的所有市场的百分比付给该交易资产的发行
  - `white_list`: 帐户必须列入白名单才能进行此操作
  - `override_authority`:发行人可以将资产转回给自己
  - `transfer_restricted`: 要求发行人是每次转移的一方
  - `disable_force_settle`: 禁用强制解决
  - `global_settle`: （仅适用于比特股）允许bitasset发布者强制全局解决 - 这可以在权限中设置，但不要设置为标志位，除非预测市场必须解决。如果已启用此标志，则不能再被借用共享！
  - `disable_confidential`:允许资产与机密交易一起使用
  - `witness_fed_asset`: 允许资产由见证人提供
  - `committee_fed_asset`: 允许资产由委员会提供
  - 启用市场费用
  - 要求持有人列入白名单
  - 发行人可以将资产转回给自己
  - 发行人必须批准所有转账
  - 禁用机密交易

如果启用UIA的*市场费用*，则必须为每个**市场交易**支付费用。这意味着，市场费用仅适用于**已填充订单**！

发行人可以更改市场的费用定义和百分比，并且发行人可以获得每个资产产生的任何累计费用。

如果市场费用设定为1％，发行人将获得1％的市场交易量作为利润。这些利润是为每个UIA积累的，发行人可以撤回。

假设，我将MyUIA市场的市场费用定为0.1％。YourUIA市场的费用为0.3％。

在比特股中，你在**收到资产**时支付费用。因此，一方将支付0.3％，另一方将支付0.1％。

## 市场锚定资产

可以!

  - `feed_lifetime_sec`:喂价的生命周期。在此之后(很快）喂价不再被视为*有效*
  - `minimum_feeds`: 数量的喂价是市场成为必需的，使其（并保持）活跃
  - `force_settlement_delay_sec`: 请求结算和实际执行结算（以秒为单位）之间的延迟
  - `force_settlement_offset_percent`: 结算价格的百分比偏差（100％= 10000）
  - `maximum_force_settlement_volume`: 每天可以结算的最大供应百分比（100％= 10000）
  - `short_backing_asset`: 必须用于*返回*此资产（借款时）


# CLI 钱包 - 常见问答

## 如何以完整的方式关闭CLI客户端？

在Windows中关闭整个窗口会产生令人讨厌的异常。在Windows中，你可以尝试使用ctrl-d来停止进程，但是仍然会产生令人讨厌的异常。

## 当我尝试运行CLI客户端时，为什么CLI客户端第一次会立即崩溃？

CLI客户端无法独立运行，即无法连接到见证人节点（通过Websocket连接）。所以要成功运行CLI客户端执行如下操作：

  - 确保在witness_node_data_dir/config.ini文件中取消注释此条目rpc-endpoint=127.0.0.1:8090
  - 在启动CLI客户端之前，你需要启动见证人节点（并等待一段时间直到它启动并运行） 
  
 <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">提示</td>
    </tr>
    <tr>
        <td bgcolor="MintCream"> 在“教程”中查找更多“如何”指南。</td>
    </tr>
    </table>


 
# 见证人 - 常见问答 

## 如何以的完整方式关闭见证人节点？  

在windows中 ``ctrl-c``。

## 如何检查见证人节点是否已同步？
 
在CLI客户端运行``info`` 命令，检查``head_block_age``的值。

## 如果它似乎超过某个日期无法同步

你应始终确保使用最新版本因为早期版本由于硬分叉将被卡住。

## 删除存储的在witness\_node\_data\_dirlogsp2p下的日志是否安全？

安全, 但是

  - 无论如何，它们会在24小时后自动循环
  - 如果你不使用它们，你应该修改``config.ini``，这样它们就不会首先写入磁盘。

## 与见证人节点交互的最佳方式是什么？

你可以与见证人节点交互的唯一方法是通过CLI客户端使用其API。
你也可以使用GUI（即轻客户端）。更多请参考：如何连接到你自己的完整节点（GUI）

## 公共和私有testnet有什么区别？

区别不大。 最大的区别是公共testnet的目的是为了更广泛参与者和想对固定（不容易改变参数），而私人可以任意设置testnets。

## 见证人节点控制台中所有这些不同文本颜色的含义是什么？

* 绿色 - 调试
* 白色 - 信息/默认  
* 黄色/棕色 - 警告
* 红色 -错误 
* 蓝色 - 一些信息, 我不是很清楚

相关的源文件位于``libraries/fc/include/fc/log/``和``libraries/fc/src/log/``中。

## 谁的私钥是“[BTS6MRyAjQ ..”，“5KQwrPbwdL ..”]？为什么在config.ini中预定义了？

这是一个特殊用途的共享密钥。但我不记得它是什么。如果我记得BM或其他人曾在论坛上解释过，但我现在找不到这个帖子了。只是让它在那里。我想如果你评论一下，它会自动出现，它是由代码witness_node生成的。


## 开发者 - 问题

# 开发者 - 常见问答

## 目前是否部署了任何分片机制？

没有

## 是否有任何特殊的隐私保护？

例如使用CoinJoin或基于ZK-SNARK的隐私方案Zerocash？如果在协议级别集成混合，你使用的是BNMCKF Mixcoin提案提出的标准

机密值（与使用相同secp256k1-zkp库的blockstream元素相同）+隐形地址。
<https://github.com/ElementsProject/elementsproject.github.io/blob/master/confidential_values.md>
没有 mixing, 没有CoinJoin.

## 协议是否为覆盖协议提供了交互机制，例如OR_RETURN？

是的, 使用自定义操作。.

``` sourceCode cpp
struct custom_operation : public base_operation
   {
      struct fee_parameters_type {
         uint64_t fee = GRAPHENE_BLOCKCHAIN_PRECISION;
         uint32_t price_per_kbyte = 10;
      };

      asset                     fee;
      account_id_type           payer;
      flat_set<account_id_type> required_auths;
      uint16_t                  id = 0;
      vector<char>              data;

      account_id_type   fee_payer()const { return payer; }
      void              validate()const;
      share_type        calculate_fee(const fee_parameters_type& k)const;
   };
```

## SPV客户端如处理的？

目前没有SPV客户端，每个完整节点都可以暴露给公众
websocket/http api。

## 如何验证交易？

每个操作都有一个定义的求值程序，它检查前置条件（do_evaluate）并修改状态（do_apply）。 （签名验证后）

``` sourceCode cpp
class transfer_evaluator : public evaluator<transfer_evaluator>
   {
      public:
         typedef transfer_operation operation_type;

         void_result do_evaluate( const transfer_operation& o );
         void_result do_apply( const transfer_operation& o );
   }
```

<table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="lightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream">请参阅系统组件列表 - 评估者信息。</td>
    </tr>
    </table>



# 新客户如何引导进入网络？

信任种子节点。了解初始见证密钥。

如何在Bitshares解决会计问题？它是Nxt风格的会计模型还是比特币的UTXO
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


每个帐户都有一组有限的余额，每种资产类型一个。


在区块链中如何解决时间问题？是使用NTP还是其他一些协议？
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


NTP

# 脚本语言是否完整？

没有脚本


# 这是通过谣传的协议还是通过中继完成的？

验证后，每个节点立即将收到的数据广播给其对等体https://github.com/cryptonomex/graphene/blob/master/libraries/p2p/design.md

# 区块链中使用了哪些数据结构？
 
    Blocks =\> transactions =\> operations =\> objects.

区块链状态包含在受影响的对象数据库中通过操作。示例对象:

    account_object
    asset_object
    account_balance_object
    ...


```
class account_balance_object : public abstract_object<account_balance_object> 
    { 
      public:
        static const uint8_t space_id = implementation_ids; 
         static const uint8_t type_id  = impl_account_balance_object_type; 
         account_id_type   owner; 
         asset_id_type     asset_type; 
         share_type        balance; 
         asset get_balance()const { return asset(balance, asset_type); }
         void  adjust_balance(const asset& delta);
   }; 
 ``` 

# 平均区块时间是多少？

当前3秒，可通过链参数进行配置。

# Bitshares交易的平均字节大小是多少？

  - 操作的平均大小约为30个字节。 
  - 操作的平均mem大小约为100个字节。

# 标准的Bitshares地址结构和格式是什么样的？

地址 = 'BTS'+base58(ripemd(sha512(compressed\_pub))) (校验消除)

但是地址不是直接使用的，而是你有一个帐户（可以由一个或多个地址，公钥或其他帐户控制）。 请参阅动态帐户权限


使用什么公钥系统？如果是椭圆曲线，那么曲线是什么？

 
像如Bitcoin, secp256k1。


# 本地支持哪些事务类型？

交易由操作组成（约40种不同类型）。操作示例如下：

  - transfer\_operation
  - limit\_order\_create\_operation
  - asset\_issue\_operation

完整列表：系统组件 - 操作

