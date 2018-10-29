::: {#build-ubuntu}

------------------------------------------------------------------------

**原文链接**：

<http://dev.bitshares.works/en/master/development/installation/build_ubuntu.html>

**译者**：https://github.com/ArcticCat1220 （北极猫）

**校对者**：

============

搭建在Ubuntu系统
================

::: {.contents}
目录
:::

------------------------------------------------------------------------

Ubuntu 16.04 LTS (64位)
-----------------------

1. 安装依赖包

:   我们建议搭建在Ubuntu 16.04 LTS
    (64位)上，搭建的依赖包可通过如下方式安装:

        sudo apt-get update
        sudo apt-get install autoconf cmake make automake libtool git libboost-all-dev libssl-dev g++ libcurl4-openssl-dev

2.  搭建BitShares Core:

        git clone https://github.com/bitshares/bitshares-core.git
        cd bitshares-core
        git checkout <LATEST_RELEASE_TAG>
        git submodule update --init --recursive
        cmake -DCMAKE_BUILD_TYPE=RelWithDebInfo .
        make

------------------------------------------------------------------------

Ubuntu 14.04 LTS (64位)
-----------------------

(*如需搭建较新版本Boost请单独阅读教程*)

1.  安装依赖包

实现纯净安装的必要步骤如下:

    sudo apt-get update
    sudo apt-get install cmake make libbz2-dev libdb++-dev libdb-dev libssl-dev openssl libreadline-dev autoconf libtool git ntp libcurl4-openssl-dev g++ libcurl4-openssl-dev

2.  搭建Boost 1.57.0

下载Boost压缩包Boost 1.57.0(\*Ubuntu 14.04适用于老版本Boost.)

::: {.注意}
1.58.0版本需要C++14并且不能搭建在Ubuntu 14.04
LTS上；这样的需求是意外产生的，详情请见发布的邮件列表。
:::

    BOOST_ROOT=$HOME/opt/boost_1_57_0
    sudo apt-get update
    sudo apt-get install autotools-dev build-essential libbz2-dev libicu-dev python-dev
    wget -c 'http://sourceforge.net/projects/boost/files/boost/1.57.0/boost_1_57_0.tar.bz2/download' -O boost_1_57_0.tar.bz2
    [ $( sha256sum boost_1_57_0.tar.bz2 | cut -d ' ' -f 1 ) == "910c8c022a33ccec7f088bd65d4f14b466588dda94ba2124e78b8c57db264967" ] || ( echo 'Corrupt download' ; exit 1 )
    tar xjf boost_1_57_0.tar.bz2
    cd boost_1_57_0/
    ./bootstrap.sh "--prefix=$BOOST_ROOT"
    ./b2 install

3.  搭建BitShares Core

<!-- -->

    cd ..
    git clone https://github.com/bitshares/bitshares-core.git
    cd bitshares-core
    git submodule update --init --recursive
    cmake -DBOOST_ROOT="$BOOST_ROOT" -DCMAKE_BUILD_TYPE=Release .
    make 

------------------------------------------------------------------------

可用软件版本
------------

### Ubuntu

我们建议搭建在Ubuntu 16.04 LTS (64位)上，搭建的依赖包可通过如下方式安装:

-   Ubuntu 16.04 LTS (**64-bit**)

::: {.注意}
比特股需要搭建在64位操作系统上，而不能搭建于32位系统。
:::

### Boost

-   Boost: 1.57到1.65之间的版本

比1.57更早或比1.65更新的版本是不被支持的。如果您系统中的Boost版本是较新版本，那么您需要用CMake的\`-DBOOST\_ROOT\`来手动安装老版本Boost。示例如下:

    cmake -DBOOST_ROOT=~/boost160 .

### OpenSSL

-   OpenSSL: 1.0.x系列

OpenSSL
1.1.0以及更新的版本是不被支持的。如果您系统中的OpenSSL版本较新，那么您需要手动提供老版本的OpenSSL。您将用到CMake的`-DOPENSSL_INCLUDE_DIR`、`-DOPENSSL_SSL_LIBRARY`和`-DOPENSSL_CRYPTO_LIBRARY`。示例如下:

    cmake -DOPENSSL_INCLUDE_DIR=/usr/include/openssl-1.0 -DOPENSSL_SSL_LIBRARY=/usr/lib/openssl-1.0/libssl.so -DOPENSSL_CRYPTO_LIBRARY=/usr/lib/openssl-1.0/libcrypto.so .

------------------------------------------------------------------------

### 常见问题:

报错：Error `{"message":"Timer Expired"}` 在Ubuntu 16.04 LTS中

如果error `{"message":"Timer Expired"}`出现了，那么原因可能是在Linux
kernel中，websocketpp\> 4.4造成的。

详见
[here](https://github.com/DECENTfoundation/DECENT-Network/issues/194)

修复步骤如下:

    cd ~/bitshares-core/libraries/fc/vendor/websocketpp
    git remote set-url origin https://github.com/DECENTfoundation/websocketpp.git
    git fetch
    git checkout 

然后搭建BitShares Core即可。

| 

| 
