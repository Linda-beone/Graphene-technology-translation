BitShares smart-coins and the pricefeed
==========================================

**原文链接**：
[https://steemit.com/bitshares/@clockwork/bitshares-smart-coins-and-the-pricefeed](https://steemit.com/bitshares/@clockwork/bitshares-smart-coins-and-the-pricefeed)

**译者**：[https://github.com/zoowii](zoowii)

**校对者**：
   
-------

如你所知，比特股交易所给用户提供了一种叫做智能代币的功能（市场化锚定资产，缩写为MPA）。

这是一种特殊的资产类型，并且不是根据个人意愿发行，而只能通过提供一定数额的抵押品（背书资产，比如BTS）从区块链中“借”出。

最经典的例子是`bitUSD`，它锚定1美元的价格，并且总是有至少1.75倍价值的抵押品，抵押品以BTS提供。（所以1 bitUSD有至少价值1.75美元的BTS做信用背书）。

MPA的具体细节，对它们存在的需求，以及交易方式的讨论已经得到了充分的讨论，并且可以通过谷歌搜索找到大量信息。

由于MPA的存在依赖一个公平的价格，它的价格通过喂价机制来提供。该价格通常由比特股区块链的见证人按有规律的周期发布到比特股区块链中。

作为一个比特股区块链的见证人，我最近开始将@xeroc的喂价脚本([https://github.com/xeroc/bitshares-pricefeed/](https://github.com/xeroc/bitshares-pricefeed/))移植到JavaScript以供我和社区的使用，并且我会持续维护。本文将讨论喂价机制。

首先，一个发布喂价的operation(译者注：比特股的交易由若干个operation构成，operation有多种代表不同业务的类型)看起来像以下这样(clarity注释过了):

  {
    "asset_id": "1.3.121", 
      // 我们提供喂价的资产类型。这个例子是bitUSD
    "extensions": [],
    "fee": {
      "amount": 57,
      // 发布喂价的operation的手续费，以BTS提供
      "asset_id": "1.3.0"                                                
    },
    "feed": {
      "core_exchange_rate": {
      // 基本汇率（当使用bitUSD来支付手续费的时候）
        "base": {
          "amount": 11930000,                                     
          // 'sats'的bitUSD数量（以最小精度计算）
          "asset_id": "1.3.121" 
        },
          // ...当购买(这种币)的时候需要这个参数...
        "quote": {                      
          "amount": 553300000, 
          // 'sats'的BTS数量（以最小精度计算）
          "asset_id": "1.3.0"
        }
      },
      "maintenance_collateral_ratio": 1750,
      "maximum_short_squeeze_ratio": 1100,
      // 这种资产的当前喂价
      "settlement_price": {                                         
        "base": {
          "amount": 16050000,
          "asset_id": "1.3.121"
        },
        "quote": {
          "amount": 781600000,
          "asset_id": "1.3.0"
        }
      }
    },
    "publisher": "1.2.711128"
    // 发布这个喂价的见证人账户
  }

维持抵押的抵押比率和最大清算时比率和保证金机制有关，可以参考这个链接得到更好的解释：
[http://docs.bitshares.org/bitshares/user/dex-margin-mechanics.html](http://docs.bitshares.org/bitshares/user/dex-margin-mechanics.html)


本质上讲，喂价行为给MPA提供了2个价格：

* a) 一个用MPA来替代BTS支付交易手续费的基本汇率。

这个值比喂价高5%（当是bitCNY时，高20%），原因是避免手续费池枯竭。

如果创建订单时用非BTS资产来支付手续费，则该手续费将隐式转换到BTS以支付网络手续费。 但是，如果订单被取消，90％的费用将作为BTS退还。 这会导致，如果基本汇率低于交易市场的最高出价，人们可以简单地从市场购买这种资产，并通过创建和取消订单隐式地与手续费池交换（BTS)。 这将耗尽手续费池并使发行人的资产收到损失（取决于基本汇率）。 因此，我们建议使用略高于资产市场价格的基本汇率。 通过这种方式，在订单中用BTS支付手续费应该总是会相对更便宜。

* b) 大部分人知道的资产喂价

你可能会好奇为什么这些价格会按这个格式发布，而不是简单的比如 0.20 USD/BTS.

原因是MPA有多个由见证人提供的喂价，并且这个区块链计算时用各喂价的中值来作为最终的喂价。 由于我们处于需要达成共识的分布式环境中，所有节点必须得出与中间喂价相同的结论。 浮点数的非确定性（参考：[https://randomascii.wordpress.com/2013/07/16/floating-point-determinism/](https://randomascii.wordpress.com/2013/07/16/floating-point-determinism/)），导致在不同的机器上进行这些计算可能得到不同结果，比如运行可能不同版本的bitshares-core， 使用不同的编译器和库版本等编译。这意味着除非我们使用整数，否则无法保证达成共识。

可以用以下方式来正确的读取以上喂价：


16050000 数值的最低精度的 bitUSD (精度为 4) ，即是

16050000 x 0.0001 = 1605USD

会给你提供

781600000 数值的最低精度的 BTS (精度为 5) ，即是

781600000 x 0.00001 = 7816BTS

得出价格: 1605/7816 = 0.2053 USD/BTS

(@xeroc的喂价脚本中有尾随的0的副作用。我在移植到JavaScript后已经优化了这部分)

但这些价格是如何计算的呢?

这由各独立的见证人根据他们使用的数据源和他们配置的喂价脚本来决定。

比如，为了计算BTS/USD交易对的价格，一个见证人可以从币安，AEX,Poloniex和火币交易所获取BTS/BTC的价格和交易量，并获得加权平均的BTS/BTC价格。

然后他们可以用同样方式从GDAX，Kraken，Bitfinex等交易所计算出BTC/USD的加权平均价格。

然后BTS/USD的价格就是BTS/BTC的价格和BTC/USD价格的乘积。

@xeroc的喂价脚本和我移植到JavaScript的版本都允许见证人创建一个配置文件，配置多个数据源以及交易对，从任意一个数据源和交易对收集喂价数据。

一旦完整的交易对列表可用，你就可以配置一个中间资产列表，以指出如何组合这些交易对来获得最终价格（在上面的示例中，BTC必须作为中间资产）以及是否允许资产的价格来自3个或2个交易所。

例如，允许bitEUR价格来自3个交易所，BTC和USD作为中间资产，这意味着可以通过BTS/BTC，BTC/USD和美元/欧元外汇汇率来计算BitEUR/BTS的潜在价格。 然后将该价格与脚本可获取的bitEUR/BTS的所有其他可能组合一起加权平均计算最终价格。

向脚本添加新数据源只需要创建一个继承FeedSource类的新类。(参考: [https://github.com/clockworkgr/bitshares-pricefeed-js/blob/master/sources/FeedSource.js](https://github.com/clockworkgr/bitshares-pricefeed-js/blob/master/sources/FeedSource.js),这里还有一个用于币安交易所的例子 [https://github.com/clockworkgr/bitshares-pricefeed-js/blob/master/sources/Binance.js](https://github.com/clockworkgr/bitshares-pricefeed-js/blob/master/sources/Binance.js))

如果见证人已经选择了他的数据源，配置和计算均价的机制，他就可以得到一个给这个MPA资产的最终公平的价格。



比如: 0.205624768946542 USD/BTS.

因为 bitUSD精度为4，我们可以四舍五入这个值为0.2056USD/BTS.

用我们前面解释过的逻辑，我们将分子和分母都按分子和分母资产的精度乘以若干个10。

然后我们现在得到了 2056 'sats'单位的bitUSD，以及 100000 'sats'单位的BTS.

所以正确发布这个价格的方式是：

    "settlement_price": {                                         
        "base": {
          "amount": 2056,
          "asset_id": "1.3.121"
        },
        "quote": {
          "amount": 100000,
          "asset_id": "1.3.0"
        }
      }

我在Javascript端口中实现的另外一个优化是首先通过最大公约数来修改这些数字。 在这种情况下，最大公约数为8，导致最终价格为：

    "settlement_price": {                                         
        "base": {
          "amount": 257, // 等于 2056/8
          "asset_id": "1.3.121"
        },
        "quote": {
          "amount": 12500, // 等于 100000/8
          "asset_id": "1.3.0"
        }
      }

我知道这篇文章有点偏技术性，但我希望它能在BitShares的喂价相关方面解决一些问题。


我的JavaScript喂价脚本仍在开发和测试中，可在此处找到: [https://github.com/clockworkgr/bitshares-pricefeed-js](https://github.com/clockworkgr/bitshares-pricefeed-js)
