项目描述

描述：

rFaaS 通过使用 ibverbs 在支持 RDMA 的网络上进行通信，实现具有个位数微秒延迟的函数调用。为了支持 InfiniBand 和 RoCE 以外的网络，我们需要一个使用 libfabric 的实现。该项目有两个主要目标：

+ 将现有代码库重构为两个使用 ibverbs 或 libfabric 的兼容实现。
+ 通过调整 TCP 或 AWS EFA 等协议的 libfabric 配置来支持更多种类的网络。

我的可能解决办法：