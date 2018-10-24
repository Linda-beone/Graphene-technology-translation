 ===========================================

  **原文链接**：<http://dev.bitshares.works/en/master/development/testnets.html>

 **译者**：https://github.com/towel223 （小办龙的蜗牛）

 **校对者**：

 ===========================================

# 如何为测试网络部署Python库 #

## 目录 ##
1.准备

2.安装

3.用法
>用Python创建市场锚定资产或用户自定义资产



1.必要条件

安装pip3用来处理包的依赖关系：


    sudo apt-get install python3-pip



2.安装

然后，我们从github获取python库并安装：

    git clone https://github.com/xeroc/python-graphenelib cd python-graphenelib/
    
    pip3 install –user -r requirements.txt python3 setup.py install –user



3.用法

这个库的用法的文档在：

https://readthedocs.org/projects/python-graphenelib/



用Python创建市场锚定资产或用户自定义资产

我们现在可以创建一些市场磨丁资产并建立喂价。


创建市场锚定资产：

https://github.com/BitSharesEurope/testnet-pythonscripts/blob/master/create_mpa.py

资金初始化见证人：

https://github.com/BitSharesEurope/testnet-pythonscripts/blob/master/fund-inits.py

喂价脚本：

https://github.com/BitSharesEurope/testnet-pythonscripts/blob/master/feed.last.py
https://github.com/BitSharesEurope/testnet-pythonscripts/blob/master/feed.parity.py
https://github.com/BitSharesEurope/testnet-pythonscripts/blob/master/feed.random.py


我们可用cron运行定期喂价脚本：

*/15 * * * * /usr/bin/python3 /home/gph/testnet-pythonscripts/feed.last.py

*/15 * * * * /usr/bin/python3 /home/gph/testnet-pythonscripts/feed.random.py

0    * * * * /usr/bin/python3 /home/gph/testnet-pythonscripts/feed.parity.py
