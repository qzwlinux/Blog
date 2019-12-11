# 如何查找连接到网络适配器的Numa节点


在某些情况下，网络适配器连接到第二个Numa节点。在第一个numa节点上运行低延迟基准（例如osu_latency）时，我们会导致其他延迟结果。

## 1.检查端口是否已连接到您的网络

    $ ibdev2netdev

    mlx5_0 port 1 ==> ib0 (Up)

    mlx5_1 port 1 ==> ib1 (Down)

在这种情况下为ib0

## 2.运行以下命令：

    $ cat /sys/class/net/ib0/device/numa_node

    1

Numa 1是第二个Numa。通常有numa 0和numa 1。

另一个可能的检查选项是通过lspci命令。

    $ lspci | grep Mel

    d8:00.0 Infiniband controller: Mellanox Technologies MT28800 Family [ConnectX-5 Ex]

    d8:00.1 Infiniband controller: Mellanox Technologies MT28800 Family [ConnectX-5 Ex]

在大多数情况下，位于PCI地址上而不以00开头的适配器连接到第二个CPU。在这种情况下，它从d8开始。

既然您已经知道适配器已连接到第二个CPU，则可以运行基准测试。