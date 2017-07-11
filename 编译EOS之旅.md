# 编译 EOS 之旅

最近在研究区块链相关的技术，作为一个 CPP 新手，和一个区块链研究新手，开始编译和学习各种区块链相关的技术项目。而 EOS 作为一个新诞生的区块链底层操作系统，令人非常有探索欲望。比起 ethereum 的处理性能， EOS 号称可以处理几十万到百万的操作。

作为探索的第一步，就是配置开发环境，以及编译构建项目，记录一下自己遇到的坑。

### ubuntu 默认 boost 版本

虚拟机跑了 ubuntu 17.04，系统默认配置了 boost ，但是版本是 1.62，然而 EOS 目前要求的 boost 版本只能是 1.64。意味着必须要手动安装 boost 。按照 boost 官网的安装方法，我图省事，直接 bootstrap.sh -prefix=/usr/ ，这样 cmake 自动会定位到 boost。

### openssl 目录

ubuntu 默认也有 openssl ，但是没有在 /usr 下面发现相关的 include 和 lib ，搜了一下也没资料。于是，就 apt install libssl-dev 。

### 指定 clang

EOS 要求 LLVM 4.0 以上，肯定是要自己安装一个 clang，但是问题是 cmake 的时候，不会提示你到底默认编译器是啥参数配置，啥情况。直接 cmake . ，大概 30% 左右就报错了。于是赶紧搜 issue ，换成了 cmake . -DCMAKE_CXX_COMPILER=/usr/bin/clang++ -DCMAKE_C_COMPILER=/usr/bin/clang -DCMAKE_CXX_STANDARD=14 , 一次编译通过。
