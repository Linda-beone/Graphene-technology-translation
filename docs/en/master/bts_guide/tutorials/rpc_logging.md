  **原文链接**：[http://dev.bitshares.works/en/master/bts_guide/tutorials/rpc_logging.html#rpc-logging](http://dev.bitshares.works/en/master/bts_guide/tutorials/rpc_logging.html#rpc-logging)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 

***   

RPC（API）日志记录
=================================

如何在BitShares中启用RPC日志记录
------------------------------------------

您可以通过在`config.ini`中附加以下行来启用它

	# declare an appender named "rpc" that writes messages to rpc.log
	[log.file_appender.rpc]
	filename=logs/rpc/rpc.log
	# filename can be absolute or relative to this config file

	# route messages sent to the "rpc" logger to the rpc appender declared above
	[logger.rpc]
	level=debug
	appenders=rpc


## 关于日志级别：

- level =错误：记录错误
- level = 警告：记录请求
- level = 信息：记录响应


		
