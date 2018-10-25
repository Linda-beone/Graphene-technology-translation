 ===========================================

  **原文链接**：<http://dev.bitshares.works/en/master/development/testnets.html>

 **译者**：https://github.com/towel223 （小办龙的蜗牛）

 **校对者**：

 ===========================================

# 代码覆盖测试 #
（本节内容来自维基百科）

## 目录 ##
>代码覆盖测试信息

>单元测试

>见证人节点

>运行特定测试

----------
>在GitHub上的比特股内核测试文件夹

# 代码覆盖测试信息 #
用gcov/lcov通过单元测试检查覆盖了多少代码(详见http:ltp.sourceforge.net/coverage/lcov.php)

    cmake -D ENABLE_COVERAGE_TESTING=true -D CMAKE_BUILD_TYPE=Debug .
    make
    lcov --capture --initial --directory . --output-file base.info --no-external
    libraries/fc/bloom_test
    libraries/fc/task_cancel_test
    libraries/fc/api
    libraries/fc/blind
    libraries/fc/ecc_test test
    libraries/fc/real128_test
    libraries/fc/lzma_test README.md
    libraries/fc/ntp_test
    tests/intense_test
    tests/app_test
    tests/chain_bench
    tests/chain_test
    tests/performance_test
    lcov --capture --directory . --output-file test.info --no-external
    lcov --add-tracefile base.info --add-tracefile test.info --output-file total.info
    lcov -o interesting.info -r total.info libraries/fc/vendor/\* libraries/fc/tests/\* tests/\*
    mkdir -p lcov
    genhtml interesting.info --output-directory lcov --prefix `pwd`

在浏览器中新打开lcov/index.html。

# 单元测试 #
我们用Boost单元测试框架来进行单元测试。大多数单元测试在chain_test中构建对象。
# 见证人节点 #
见证人的任务是广播处理，打包区块，并随机签署证明这些操作。

    ./witness_node --rpc-endpoint 127.0.0.1:8090 --enable-stale-production -w '"1.6.0"' '"1.6.1"' '"1.6.2"' '"1.6.3"' '"1.6.4"' '"1.6.5"' '"1.6.6"' '"1.6.7"' '"1.6.8"' '"1.6.9"' '"1.6.10"' '"1.6.11"' '"1.6.12"' '"1.6.13"' '"1.6.14"' '"1.6.15"' '"1.6.16"' '"1.6.17"' '"1.6.18"' '"1.6.19"' '"1.6.20"' '"1.6.21"' '"1.6.22"' '"1.6.23"' '"1.6.24"' '"1.6.25"' '"1.6.26"' '"1.6.27"' '"1.6.28"' '"1.6.29"' '"1.6.30"' '"1.6.31"' '"1.6.32"' '"1.6.33"' '"1.6.34"' '"1.6.35"' '"1.6.36"' '"1.6.37"' '"1.6.38"' '"1.6.39"' '"1.6.40"' '"1.6.41"' '"1.6.42"' '"1.6.43"' '"1.6.44"' '"1.6.45"' '"1.6.46"' '"1.6.47"' '"1.6.48"' '"1.6.49"' '"1.6.50"' '"1.6.51"' '"1.6.52"' '"1.6.53"' '"1.6.54"' '"1.6.55"' '"1.6.56"' '"1.6.57"' '"1.6.58"' '"1.6.59"' '"1.6.60"' '"1.6.61"' '"1.6.62"' '"1.6.63"' '"1.6.64"' '"1.6.65"' '"1.6.66"' '"1.6.67"' '"1.6.68"' '"1.6.69"' '"1.6.70"' '"1.6.71"' '"1.6.72"' '"1.6.73"' '"1.6.74"' '"1.6.75"' '"1.6.76"' '"1.6.77"' '"1.6.78"' '"1.6.79"' '"1.6.80"' '"1.6.81"' '"1.6.82"' '"1.6.83"' '"1.6.84"' '"1.6.85"' '"1.6.86"' '"1.6.87"' '"1.6.88"' '"1.6.89"' '"1.6.90"' '"1.6.91"' '"1.6.92"' '"1.6.93"' '"1.6.94"' '"1.6.95"' '"1.6.96"' '"1.6.97"' '"1.6.98"' '"1.6.99"' '"1.6.100"'
    
# 运行特定测试 #


- tests/chain_tests -t block_tests/name_of_test