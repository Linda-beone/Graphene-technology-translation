.. _build-osx:

===========================================

 **原文链接**：<http://dev.bitshares.works/en/master/development/installation/build_osx.html>

**译者**：https://github.com/ArcticCat1220 （北极猫）

**校对者**：

============

搭建在OS X系统
==============

::: {.contents}
目录
:::

------------------------------------------------------------------------

比特股OS X系统搭建教程
----------------------

1.  安装XCode及其命令行工具

**教程** 见链接： <https://guide.macports.org/#installing.xcode>.

在OS X 10.11 (El
Capitan)和更新的版本中，当在终端运行开发者指令时，您将被提示安装开发者工具。但这一步骤并不是必须的。

2.  安装Homebrew

**教程** 见链接: <http://brew.sh/>

3.  初始化Homebrew:

        brew doctor
        brew update

4.  安装依赖包:

        brew install boost cmake git openssl autoconf automake 
        brew link --force openssl 

5.  (可选) 用于支持比特币钱包文件:

        brew install berkeley-db

6.  (可选) 如需在LevelDB中使用TCMalloc:

        brew install google-perftools

7.  克隆比特股仓库:

        git clone https://github.com/bitshares/bitshares-core.git
        cd bitshares-core

8.  搭建比特股:

        git submodule update --init --recursive
        cmake .
        make

------------------------------------------------------------------------

可用版本
--------

### Boost

-   Boost：1.57版本至1.65版本

您可以通过brew查询boost版本:

    brew search boost

如需安装另一个版本的Boost(如1.60版本):

    brew install boost@1.60

### OpenSSL

-   OpenSSL: 1.0.x series

如果您的OpenSSL版本较旧不符合要求的话，可以通过brew来获取最新版本:

    brew upgrade openssl

编译这些新版本：现在我们必须告诉cmake这些库的位置。不同于之前提到的\"cmake
.\"，这里我们将用到:

    cmake -DBOOST_ROOT=/usr/local/opt/boost@1.60 -DOPENSSL_ROOT_DIR=/usr/local/opt/openssl .

然后以正常方式运行:

    make

| 
