**原文链接**：[https://dev.bitshares.works/en/master/knowledge_base/wiki_legacy/howto_import_bitShares_09_delegate.html](https://dev.bitshares.works/en/master/knowledge_base/wiki_legacy/howto_import_bitShares_09_delegate.html)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***

如何在BitShares 2.0中将现有委托作为见证导入
=========================================================================

在BitShares 0.9网络的准备工作
--------------------------------------------

我们需要从BitShares 0.9中提取签名的公钥和私钥。

让我们获取&lt;publickey&gt;:

    >>> get_account <delegatename>
    [...]
    Block Signing Key: <publickey>
    [...]

备注：BitShares网络中的公钥具有前缀BTS。 因此，在石墨烯测试网的情况下，你应该用GPH替换BTS。

和相应的&lt;wifkey&gt;:

    >>> wallet_dump_account_private_key <delegatename> signing_key
    "<wifkey>"
