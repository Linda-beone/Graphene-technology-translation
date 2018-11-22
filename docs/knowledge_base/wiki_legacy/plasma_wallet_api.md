  **原文链接**：[https://dev.bitshares.works/en/master/knowledge_base/wiki_legacy/plasma_wallet_api.html](https://dev.bitshares.works/en/master/knowledge_base/wiki_legacy/plasma_wallet_api.html)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***


Plasma 钱包 API
========================

Plasma 钱包 API
---------------------------

此API定义了开发人员如何使用Plasma创建和管理钱包。 钱包是可以存储私人用户信息的地方。 此信息以加密的方式储存在磁盘上或在远程服务器上。

此API的目的是抽象同步本地钱包和远程副本的详细信息。

假设一个名为** Wallet **的类具有以下调用：

此方法将配置钱包以查找远程主机加载和/或保存你的钱包。 通过将null传入此调用，钱包将停止与远程服务器同步其状态。

    wallet.useBackupServer( url )

此调用用于配置钱包以在磁盘上保留本地副本。 即使服务器不可用，这也允许用户可以访问钱包。 可以在钱包数据永远不会触及磁盘的公共计算机上禁用此选项，并且应在用户注销时将其删除。

    wallet.keepLocalCopy( save = true )

此方法用于配置钱包将其数据保存在远程服务器上。 如果将其设置为false，则将从服务器中删除它。 如果设置为true，则将其上载到服务器。 如果钱包当前未保存在服务器上，则需要令牌以允许创建新钱包。

    wallet.keepRemoteCopy( save = true, token = null )

此API调用用于加载钱包。 如果已指定备份服务器，则它将尝试从服务器获取最新版本，否则它将将本地钱包加载到内存中。 keepLocalCopy设置的配置将确定钱包是否作为登录的副作用保存到磁盘。当钱包在RAM中解锁时，它将如下组合解锁：小写（电子邮件）+小写（用户名）+密码来使用匹配的公钥/私钥。 如果启用了keepRemoteCopy，则用于获取代币的电子邮件必须与此处使用的电子邮件匹配。 此外，如果启用了keepRemoteCopy，则服务器将仅存储电子邮件的单向哈希（而不是电子邮件本身），以便它可以通过唯一的电子邮件跟踪资源。

    wallet.login( email, username, password )

此API调用将从内存中删除钱包状态。

    wallet.logout()

此方法返回表示钱包状态的JSON对象。 仅在钱包已成功登录时才有效。

    wallet.getState()

此方法用于更新钱包状态。 如果钱包配置为与远程钱包保持同步，则服务器将引用钱包修订历史记录的副本，以确保不会覆盖任何版本。 如果本地钱包落在分叉上，任何上传该钱包的尝试都将导致API调用失败。 应该提醒用户，以便他们可以手动调整。 在服务器上成功存储状态后，将状态保存到本地和可选的硬盘上。

    wallet.setState( state ) 
