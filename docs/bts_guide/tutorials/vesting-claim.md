申领授权资产
===========================================

**原文链接**：
[http://dev.bitshares.works/en/master/bts_guide/tutorials/vesting-claim.html](http://dev.bitshares.works/en/master/bts_guide/tutorials/vesting-claim.html)

**译者**：[https://github.com/zoowii](zoowii)

**校对者**：

---------

# 命令行钱包

在命令行钱包，可以通过以下命令来申领来自见证人的授权资产：


    withdraw_vesting <账户名> <金额> <资产>

不幸的是，不存在现成的命令来申领不是见证人支付的授权资产。尽管如此，可以参考章节 `手动构造交易`来构造带有`vesting_balance_withdraw_operation`这个operation的交易。交易结构形如：


    [
      33,{
        "fee": {
          "amount": 0,
          "asset_id": "1.3.0"
        },
        "vesting_balance": "1.13.0",
        "owner": "1.2.0",
        "amount": {
          "amount": 0,
          "asset_id": "1.3.0"
        }
      }
    ]

		