# Linux系统调整

您可以使用Linux sysctl命令来修改操作系统设置的默认系统网络参数，以提高IPv4和IPv6流量性能。但是请注意，更改网络参数可能会在不同的系统上产生不同的结果。结果很大程度上取决于CPU和芯片组的效率。

## 调整网络适配器以提高IPv4流量性能

建议进行以下更改以提高IPv4流量性能：

1.禁用TCP时间戳选项以提高CPU利用率：

    ＃sysctl -w net.ipv4.tcp_timestamps = 0

2.启用“ TCP选择性确认”选项以提高吞吐量：

    ＃sysctl -w net.ipv4.tcp_sack = 1

3.增加处理器输入队列的最大长度： 

    ＃sysctl -w net.core.netdev_max_backlog = 250000

4.使用setsockopt（）增加TCP的最大和默认缓冲区大小：

    ＃sysctl -w net.core.rmem_max = 4194304

    ＃sysctl -w net.core.wmem_max = 4194304

    ＃sysctl -w net.core.rmem_default = 4194304

    ＃sysctl -w net.core.wmem_default = 4194304

    ＃sysctl -w net.core.optmem_max = 4194304

5.增加内存阈值以防止数据包丢失：

    ＃sysctl -w net.ipv4.tcp_rmem =“ 4096 87380 4194304”

    ＃sysctl -w net.ipv4.tcp_wmem =“ 4096 65536 4194304”

6.为TCP启用低延迟模式：

    ＃sysctl -w net.ipv4.tcp_low_latency = 1

以下变量用于告诉内核应将多少套接字缓冲区空间用于TCP窗口大小，以及为应用程序缓冲区节省多少。

    ＃sysctl -w net.ipv4.tcp_adv_win_scale = 1

值为1表示套接字缓冲区将在TCP窗口大小和应用程序之间平均分配。

## 重新启动后保留系统设置

要在重启后保留性能设置，您需要将其添加到文件中

/etc/sysctl.conf如下：

    <sysctl name1>=<value1>

    <sysctl name2>=<value2>

    <sysctl name3>=<value3>

    <sysctl name4>=<value4>

例如，“调整网络适配器以提高IPv4流量性能”（第16页）列出了以下设置以禁用TCP时间戳选项：

    ＃sysctl -w net.ipv4.tcp_timestamps = 0

为了使重新启动后禁用TCP时间戳选项，请将以下行添加到

/etc/sysctl.conf：

    net.ipv4.tcp_timestamps = 0