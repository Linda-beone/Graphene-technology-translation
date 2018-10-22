.. _installation-guide:

===========================================

 **原文链接**：<http://dev.bitshares.works/en/master/development/installation/boost_versions.html>

**译者**：https://github.com/ArcticCat1220 （北极猫）

**校对者**：

============

Boost版本
=========

支持的版本
----------

-   Boost: 1.57至1.65

早于1.57的版本或晚于1.65的版本是不被支持的。如果您系统的Boost版本较新，那么您需要手动搭建老版本Boost并使用CMake的-DBOOST\_ROOT。例如:

    cmake -DBOOST_ROOT=~/boost160 .

------------------------------------------------------------------------

常见问题
--------

### bitshares-core

\- 如果搭建的是\**boost 1.60*\*
(或者其它可能的版本)，那么一些参数类型在命令行是不被支持的。 \#881
(例如)

    - `./cli_wallet -s ws://1.2.3.4:5678/`可在boost 1.57和1.58版本使用，但在1.60版本是不可行的
    - `./cli_wallet -sws://1.2.3.4:5678/` 可在boost 1.57、1.58和1.60中通用

-  
    [time\_point\_sec]{.title-ref} 内部使用无符号整型变量来计数从纪元开端起的秒数，像unix时间一样。这意味着1970-01-01前的日期是不可能被呈现的。同样的，有时候我们看到在JSON输出中出现早于1970-01-01的日期，很可能是一个boost漏洞::posix\_time, 目前已经接到报告称在Boost-1.64\#597版本中，这一问题已被修复。

-   更新至boost 1.64版本可能会修复这一问题，然而，目前的BitShares还暂时不适用于Boost 1.64版本(录入时间:4/29/2018)

### bitshares-fc

| 
