  **原文链接**：[http://dev.bitshares.works/en/master/bts_guide/tutorials/publish-feed.html#publish-feed](http://dev.bitshares.works/en/master/bts_guide/tutorials/publish-feed.html#publish-feed)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 

***    

发布喂价
==========================

如何发布喂价
----------------------------------------------

喂价操作采用以下形式：

    {
      "publisher": "1.2.0",
      "asset_id": "1.3.0",
      "feed": {
        "settlement_price": {
          "base": {
            "amount": 0,
            "asset_id": "1.3.0" },
          "quote": {
            "amount": 0,
            "asset_id": "1.3.0" }
        },
        "maintenance_collateral_ratio": 1750,
        "maximum_short_squeeze_ratio": 1200,
        "core_exchange_rate": {
          "base": {
            "amount": 0,
            "asset_id": "1.3.0" },
          "quote": {
            "amount": 0,
            "asset_id": "1.3.0" }
        }
      }
     }

它包含`发行者`名称，为其生成`喂价`的`asset_id`，作为`基数`和`报价`比率的结构，维护抵押比率（1750 = 175％）， 轧空比率（1200 = 120％）和隐含交换费用的核心交易汇率。

Python 脚本示例

    from grapheneapi import GrapheneClient
    import json


    class Config():
        wallet_host           = "localhost"
        wallet_port           = 8092
        wallet_user           = ""
        wallet_password       = ""

    if __name__ == '__main__':
        graphene = GrapheneClient(Config)

        price = 1.50  # one BTS costs 1.50 SYMBOLs
        asset_symbol = "SYMBOL.BIT"
        producer = "nathan"


        account = graphene.rpc.get_account(producer)
        asset = graphene.rpc.get_asset(asset_symbol)                                                                       
        base = graphene.rpc.get_asset("1.3.0")                                                                             
        price = price * 10 ** asset["precision"] / 10 ** base["precision"]                                                 
        denominator = base["precision"]                                                                                    
        numerator   = round(price*base["precision"])
        price_feed  = {"settlement_price": {
                       "quote": {"asset_id": "1.3.0",
                                 "amount": denominator
                                 },
                       "base": {"asset_id": asset["id"],
                                "amount": numerator
                                }
                       },
                       "maintenance_collateral_ratio" : 1750,
                       "maximum_short_squeeze_ratio"  : 1300,
                       "core_exchange_rate": {
                       "quote": {"asset_id": "1.3.0",
                                 "amount": int(denominator * 1.05)
                                 },
                       "base": {"asset_id": asset["id"],
                                "amount": numerator
                                }}}
        handle = graphene.rpc.begin_builder_transaction()
        op = [19,  # id 19 corresponds to price feed update operation
              {"asset_id"  : asset["id"],
               "feed"      : price_feed,
               "publisher" : account["id"]
               }]
        graphene.rpc.add_operation_to_builder_transaction(handle, op)
        graphene.rpc.set_fees_on_builder_transaction(handle, "1.3.0")
        tx = graphene.rpc.sign_builder_transaction(handle, True)
        print(json.dumps(tx, indent=4))
