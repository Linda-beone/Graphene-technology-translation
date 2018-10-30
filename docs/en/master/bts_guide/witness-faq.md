 **原文链接**：[http://dev.bitshares.works/en/master/bts_guide/witness-faq.html#witness-faq-1](http://dev.bitshares.works/en/master/bts_guide/witness-faq.html#witness-faq-1)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***
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
