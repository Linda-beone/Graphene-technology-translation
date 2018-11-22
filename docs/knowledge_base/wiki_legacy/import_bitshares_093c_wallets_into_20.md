  **原文链接**：[https://dev.bitshares.works/en/master/knowledge_base/wiki_legacy/import_bitshares_093c_wallets_into_20.html](https://dev.bitshares.works/en/master/knowledge_base/wiki_legacy/import_bitshares_093c_wallets_into_20.html)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***

将BitShares 0.9.3c钱包导入BitShares 2.0 CLI钱包
==============================================================

如何从BitShares 0.9.3导入大钱包
-----------------------------------------------------

在bitshares 0.9.3c中，运行:

    wallet_export_keys /tmp/final_bitshares_keys.json

在bitshares 2.0 CLI钱包中，运行：>>> import_accounts /tmp/final_bitshares_keys.json my_password

然后，为你钱包中的每个帐户（运行list_my_accounts以查看它们）:

    >>> import_account_keys /tmp/final_bitshares_keys.json my_password my_account_name my_account_name

注意：在发布标记中，这将在导入每个密钥后创建钱包的完整备份。 如果你有成千上万的密钥，这很慢，也占用了大量的磁盘空间。 在导入期间监视可用磁盘空间，并在必要时定期擦除备份以避免填满磁盘。 最新代码仅在导入所有密钥后保存你的钱包：

      >>> import_balance my_account_name ["*"] true
      >>> list_account_balances my_account_name

验证结果
