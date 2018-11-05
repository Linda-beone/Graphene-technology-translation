Bitshares ElasticSearch Wrapper and Swagger
===========================================

**原文链接**：
[https://steemit.com/bitshares/@oxarbitrage/bitshares-elasticsearch-wrapper-and-swagger](https://steemit.com/bitshares/@oxarbitrage/bitshares-elasticsearch-wrapper-and-swagger)

**译者**：[https://github.com/zoowii](zoowii)

**校对者**：
   
-------

这是一个在比特股中使用elasticsearch的尝试：[https://github.com/oxarbitrage/bitshares-es-wrapper](https://github.com/oxarbitrage/bitshares-es-wrapper)

已经给API添加了Swagger的API文档：[http://185.208.208.184:5000/apidocs/#!/wrapper/](http://185.208.208.184:5000/apidocs/#!/wrapper/)


这个功能是通过安装flasgger([https://github.com/rochacbruno/flasgger](https://github.com/rochacbruno/flasgger))并在elasticsearch-wrapper中简单实现的。

你可以通过以下git commit中看到如何在Flask应用中添加flasgger的支持：
[https://github.com/oxarbitrage/bitshares-es-wrapper/commit/89ed7c036dd83a530995641ae186fbcabbf308c5](https://github.com/oxarbitrage/bitshares-es-wrapper/commit/89ed7c036dd83a530995641ae186fbcabbf308c5)

你只需要在这个项目的这个位置([https://github.com/oxarbitrage/bitshares-es-wrapper/blob/master/wrapper.yaml](https://github.com/oxarbitrage/bitshares-es-wrapper/blob/master/wrapper.yaml))编辑yaml文件就可以完成（使用这个工具）需要的所有工作。

这个项目还需要更多的开发工作，但是我计划实现其他规范的API：
[https://github.com/oxarbitrage/bitshares-python-api-backend](https://github.com/oxarbitrage/bitshares-python-api-backend)
