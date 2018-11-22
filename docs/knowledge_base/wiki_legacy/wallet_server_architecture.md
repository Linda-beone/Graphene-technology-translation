  **原文链接**：[https://dev.bitshares.works/en/master/knowledge_base/wiki_legacy/wallet_server_architecture.html](https://dev.bitshares.works/en/master/knowledge_base/wiki_legacy/wallet_server_architecture.html)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***

钱包服务器架构
==================================

为了保持与任何其他网站类似的用户体验，用户可以登录网站并从任何计算机访问其私钥，这一点至关重要。同样重要的是服务器无法访问用户的私钥。

该规范的目标是在Web浏览器和服务器之间定义用于存储和检索用户的私有数据的协议。由于所有数据都是加密的，因此服务器需要使用一些技术来防止滥用。我们假设在将数据存储在服务器上之前，用户必须请求将通过电子邮件发送给他们的代码。

以下语法是发送到服务器或从服务器发送的JSON-RPC请求的简写：

    server.requestCode( email )

然后用户可以在服务器上存储数据，如下所示：

    server.createWallet( code, encrypted_data, signature ) 

钱包将使用从encrypted_data和signature派生的public_key保存在服务器上。该代码包含电子邮件的不可逆哈希值。这用于唯一约束，有效地将用户限制为每个电子邮件一个钱包。原始电子邮件地址未存储在数据库中。

然后用户可以从服务器获取数据，如下所示：

    server.fetchWallet( publickey, local_hash )

如果钱包与已在本地缓存的钱包不同，则此方法仅获取钱包。除了返回加密钱包之外，此方法还将返回有关上次更新时间以及哪些IP已请求钱包以及何时更新的统计信息。用户可以使用它来检测潜在的欺诈。

用户可以使用此调用更新其数据：

    server.saveWallet( original_local_hash, encrypted_data, signature )

签名可用于导出应存储`encrypted_data`的公钥。服务器必须验证数据库中是否存在派生公钥，否则此方法将失败。如果`original_local_hash`与被覆盖的钱包不匹配，则此方法需要失效。

用户可以通过以下调用更改其“密钥”：

    server.changePassword( original_local_hash, original_signature, new_encrypted_data, new_signature )

在此调用之后，用于查找此钱包的公钥将是从`new_signature`和`new_encrypted_data`派生的公钥。钱包必须存在于从`original_local_hash`和`original_signature`派生的`old_public_key`中。

用户可以通过以下调用删除其钱包：

    server.deleteWallet( local_hash, signature )

出于安全原因，此删除是永久性的。用户可以使用相同的电子邮件重复该过程创建新钱包。

生成公钥
--------------------------

用户从其密码生成私钥。一般而言，密码太弱并且可以通过与用户独有且用户不太可能忘记的其他信息组合来加强。至少可以将电子邮件地址和/或用户名与密码组合以恢复钱包的私钥。其他信息（如社会安全号码，护照ID，电话号码或其他安全问题的答案）可能对某些应用程序有用。请记住，用户必须记住他们的密码和确切的安全问题和答案才能恢复他们的帐户。

减少暴力攻击
---------------------------------------------

服务器应将每个IP地址接受的钱包请求数限制为每小时固定数量。通过这种方式，除非服务器本身受到入侵，否则用户钱包数据将免于遭受暴力攻击。如果服务器遭到入侵，用户只受其密码质量和用作salt的任何其他数据的保护。

在本地保存钱包
-----------------------------

浏览器应该在本地缓存钱包数据

