.. _installation-guide:

===========================================

 **原文链接**：
<http://dev.bitshares.works/en/master/development/installation.html>

**译者**：https://github.com/ArcticCat1220 （北极猫）

**校对者**：

安装
----

BitShares Core是比特股区块链的实现和命令行的接口。Web端钱包是 [BitShares
UI](https://github.com/bitshares/bitshares-ui)。

您可以访问 [BitShares.org](https://bitshares.org/)
来学习比特股相关知识和加入我们的社区
[BitSharesTalk.org](https://bitsharestalk.org/)。

**提示:**
官方的比特股git仓库地址、默认分支和子模块远程控制等近期都已变更。现存的仓库可以通过如下步骤更新：

> git remote set-url origin
> <https://github.com/bitshares/bitshares-core.git> git checkout master
> git remote set-head origin \--auto git pull git submodule sync
> \--recursive git submodule update \--init \--recursive

::: {.注意}
查看比特股系统需求
`system requirements <system-requirements-node>`{.interpreted-text
role="ref"}.
:::

### 下载并搭建

选择操作系统并安装和搭建。

::: {.toctree}
installation/build\_ubuntu installation/build\_osx
installation/build\_windows installation/build\_windows\_devenv
installation/windows\_cli\_tool
:::

### 在WSL搭建和运行BitShares-Core(安装选项)

::: {.toctree}
installation/wsl
:::

### 常见问题

::: {.toctree}
installation/boost\_versions
:::

------------------------------------------------------------------------

| 
