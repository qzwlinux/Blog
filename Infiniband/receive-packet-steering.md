# 接收数据包指导

与选择队列并因此选择运行硬件中断处理程序的CPU的RSS相反，RPS选择要在中断处理程序之上执行协议处理的CPU。

RPS需要使用CONFIG_RPS kconfig符号编译的内核（SMP默认情况下为ON）。即使在编译时，RPS仍保持禁用状态，直到明确配置。

可以使用sysfs文件条目为每个接收队列配置RPS可能会将流量转发到的CPU列表：

    /sys/class/net/<dev>/queues/rx-<n>/rps_cpus

对于具有单个队列或其队列的数量少于NUMA节点核心数量的接口，建议将rps_cpus掩码配置为设备NUMA节点核心列表，以获得更好的多队列接口并行性。

例：

在“连接”模式下使用IPoIB时，它只有一个rx队列。

要启用RPS：

    # cat /sys/class/net/ib0/device/local_cpus

    0000,00000000,00000000,00fffc00,0fffc000

    # cat /sys/class/net/ib0/queues/rx-0/rps_cpus

    0000,00000000,00000000,00000000,00000000

    # LOCAL_CPUS=`cat /sys/class/net/ib0/device/local_cpus`

    # echo $LOCAL_CPUS > /sys/class/net/ib0/queues/rx-0/rps_cpus

