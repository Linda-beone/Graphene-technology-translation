  **原文链接**：[http://how.bitshares.works/en/master/bts_holders/tokens/privbta.html](http://how.bitshares.works/en/master/bts_holders/tokens/privbta.html)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***


2.4.私有化BitAssets
=======================

除了像比特美元这样的常规市场锚定资产之外，BitShares还为企业家提供了一个机会，可以使用自定义参数和一组独特的喂价生成器创建自己的智能货币。

**私有化智能货币**经理可以尝试不同的参数，如抵押要求，喂价，强制结算延迟和强制结算费用。他们还从已发行资产所涉及的交易中获得交易费用，因此具有在网络上营销和推广它的财务激励。能够发现和推广最佳参数组的企业家可以获得巨大的利润。企业家可以调整的参数集足够广泛使得智能货币可用于实现功能完善的预测市场，以合理的价格保证全球结算，并且在解决日期之前不会强制结算。

一些企业家可能想要尝试智能货币，它总是以1.00美元的价格交易，而不是严格超过1.00美元。他们可以通过持续操纵强制结算费来做到这一点，使平均交易价格保持在1.00美元左右。默认情况下，BitShares更喜欢市场设定的费用，因此选择让价格浮动超过1.00美元，而不是通过直接操纵强制结算费来固定价格。

准备工作
---------------------

* 参考:资产常见问题解答 <asset-faq-index>
* 参考:如何手动创建UIA（CLI）? <uia-create-manual>
* 参考:如何手动创建MPA（CLI）? <mpa-create-manual>


## 参数

相关且有趣的参数位于uia标志中：

   {
      "witness_fed_asset" : false
      "committee_fed_asset" : false
   }


将这两个参数设置为“false”，允许手动定义一组Feed生成器（见下文）。 或者，将两者中的任何一个设置为“true”将使相应的实体有责任生成和发布Feed。

改变feed生产者
------------------------------

以下命令将替换新的Feed生成器集合中当前允许的Feed生成器集：


    update_asset_feed_producers <symbol> ["prod-a", "prod-b"] True

生产feed
------------------------

我们有一个独特的教程，描述了如何发布Feed。
 
* 参考:如何发布Feed价格（CLI）？<publish-feed>
