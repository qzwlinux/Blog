# 如何调整AMD服务器（EYPC CPU）以获得最佳性能

[**link**](https://community.mellanox.com/s/article/how-to-tune-an-amd-server--eypc-cpu--for-maximum-performance)

## 验证系统配置

在进行CPU调整之前，我们必须检查NUMA节点配置，并验证我们的服务器是否实际在运行AMD CPU：

    # lscpu

    Architecture: x86_64

    CPU op-mode(s): 32-bit, 64-bit

    Byte Order: Little Endian

    ...

    Thread(s) per core: 1

    Core(s) per socket: 32

    Socket(s): 1

    NUMA node(s): 4

    Vendor ID: AuthenticAMD

    ...

    Model name: AMD EPYC 7551 32-Core Processor

    ...

    CPU MHz: 1996.203

    ...

    NUMA node0 CPU(s): 0,4,8,12,16,20,24,28

    NUMA node1 CPU(s): 1,5,9,13,17,21,25,29

    NUMA node2 CPU(s): 2,6,10,14,18,22,26,30

    NUMA node3 CPU(s): 3,7,11,15,19,23,27,31

    ...

在上面的输出中，我们可以观察到被测试的服务器正在运行AMD CPU 模式l具有四个八核 NUMA节点的“ EPYC 7551 32核处理器” 。

由于禁用了超线程，因此总共只有32个CPU（物理和逻辑）可用。

要查找Mellanox NIC的本地 NUMA节点，请参考以下操作方法：[了解NUMA节点的性能基准](https://community.mellanox.com/s/article/understanding-numa-node-for-performance-benchmarks)

在此示例中，我们将把Mellanox NIC的本地节点调整为NUMA节点＃2。为此，请运行：

    ＃cat /sys/class/net/eth20/device/numa_node

    2

对于性能调整过程，我们将使用本地CPU内核2,6,10,14,18,22,26,30。

## 性能调优

为了最大化NIC的带宽，中断事件处理必须仅由本地CPU处理。这将本地化处理和内存使用情况，并减少QPI开销。


## 重点
请参阅[什么是IRQ Affinity](https://community.mellanox.com/s/article/what-is-irq-affinity-x)？想要查询更多的信息。

要将NIC的中断事件绑定到本地核心，请运行：

    # service irqbalance stop

    # set_irq_affinity_cpulist.sh 2,6,10,14,18,22,26,30 eth20

或者，可以使用mlnx_tune工具（将在所有Mellanox NIC上自动运行）将NIC的中断事件绑定到本地内核，运行：

    ＃mlnx_tune -p HIGH_THROUGHPUT

## 性能测试结果

以下是针对以下设置进行上述调整后的预期OOB结果：

- iperf
- 8 threads
- TCP window 512KB
- 8KB message size

Server:

    iperf -s

Client:

    iperf -c 120.7.84.141 -P 8 -t 10 -w 512k

 

Expected results:

MTU|Tuning|Bandwith
---|:--:|---
1500B|OOB|~50Gb/s
1500B|Tuned|~90Gb/s
9000B|OOB|~85Gb/s
9000B|Tuned|~97.5Gb/s