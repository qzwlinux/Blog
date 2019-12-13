# Infiniband笔记

技术|全拼|速度
---|:--:|---
SRD|SingleDataRate|8Gb/s|
DDR|DoubleDataRate|16Gb/s
QDR|QuadDataRate|32Gb/s
FDR|FourteenDataRate|56Gb/s
EDR|EnhancedDataRate|100Gb/s
HDR|HighDataRate|200Gb/s
NDR|NextDataRate|1000Gb/s

## 系统设置

### [Bios调优](./Bios调优.md)

- 通过优化Bios选项来提升Infiniband速度

### [了解PCIe以获得最佳性能](./PCIe_Performance.md)

- Pcie Gen3
- Pcie x4 ---> x8 ---> x16 速度会快

### [了解NUMA节点的性能基准](./NUMA_Performance.md)

- mst status -v

### [了解如何检查CPU核心频率](./CPU_Frequency.md)

### [如何将CPU扩展调节器设置为最大性能](./CPU_Performance.md)

### [如何调整AMD服务器（EYPC CPU）以获得最佳性能](./RUN_IN_AMD.md)


### [如何查找连接到网络适配器的NUMA节点](./NUMA_NETWORK.md)


## 操作系统和驱动

### Affinity/Interrupt（非重点）

- [What is CPU Affinity?](https://community.mellanox.com/s/article/what-is-cpu-affinity-x)

- [What is IRQ Affinity?](https://community.mellanox.com/s/article/what-is-irq-affinity-x)

### Interrupt Moderation（非重点）

- [Understanding Interrupt Moderation](https://community.mellanox.com/s/article/understanding-interrupt-moderation)

- [Dynamically-Tuned Interrupt Moderation (DIM)](https://community.mellanox.com/s/article/dynamically-tuned-interrupt-moderation--dim-x)


### [ConnectX-3/Pro Tuning For Linux (Idle Loop and IP Forwarding)](https://community.mellanox.com/s/article/connectx-3-pro-tuning-for-linux--idle-loop-and-ip-forwarding-x)

### [接受数据包指导](./receive-packet-steering.md)

### [Linux系统调优](./Linux_systctl_tuning.md)

### [Rivermax Linux性能调优](https://community.mellanox.com/s/article/rivermax-linux-performance-tuning-guide--1-x)

### [如何使用mlnx_tune工具调整Linux服务器以获得最佳性能](https://community.mellanox.com/s/article/How-to-Tune-Your-Linux-Server-for-Best-Performance-Using-the-mlnx-tune-Tool)

## Infiniband性能测试

下面的文章将向您介绍OFED的性能测试（perftest）程序包

perftest软件包包含一组带宽和延迟基准

InfiniBand / RoCE

- [ib_send_bw](./ib_send_bw.md)
- [ib_send_lat](./ib_send_lat.md)
- [ib_write_bw](./ib_write_bw.md)
- [ib_write_lat](./ib_write_lat.md)
- [ib_read_bw](./ib_read_bw.md)
- [ib_read_lat](./ib_read_lat.md)
- [ib_atomic_bw](./ib_atomic_bw.md)
- [ib_atomic_lat](./ib_atomic_lat.md)

本机以太网

- [raw_ethernet_bw](./raw_ethernet_bw.md)
- [raw_ethernet_lat](./raw_ethernet_lat.md)
- [raw_ethernet_burst_lat](./raw_ethernet_burst_lat.md)


[**划重点Performance**](https://community.mellanox.com/s/article/howto-enable--verify-and-troubleshoot-rdma
)
## Nvme

### [Ethtool](https://community.mellanox.com/s/article/understanding-mlx5-ethtool-counters)

### ip link & ifconfig

ip link和ifconfig是网络设备配置和监视工具。

- 所有ip链接和ifconfig监视和配置均适用于RDMA。
- 其他相关的监视和配置工具：
    - gids-使用show_gids工具（[**info**](https://community.mellanox.com/s/article/understanding-show-gids-script)）
    - ib info-使用ibv_devinfo（[**手册页**](./ibv_info.md)）
    - rdma计数器，包括拥塞控制-在/sys/class/infiniband/和/sys/class/net位置下（[[**info**](https://community.mellanox.com/s/article/understanding-mlx5-linux-counters-and-status-parameters)）
    - 拥塞配置-存储在设备的非易失性内存中。如果需要调优，建议寻求Mellanox支持。可以在[**这里**](https://community.mellanox.com/s/article/dcqcn-parameters)找到说明。
    - ibdev2netdev 此命令查看卡对应网卡名

### tcpdump 以及 RDMA测试

转储网络流量：

注意：对于RDMA，请使用ibdump（[**info**](https://community.mellanox.com/s/article/howto-enable--verify-and-troubleshoot-rdma)）

### iperf3
测试网络吞吐量。

注：对于RDMA，请使用ib_send_bw（[**info**](https://community.mellanox.com/s/article/ib-send-bw)）和ib_send_lat（[**info**](https://community.mellanox.com/s/article/ib-send-lat)）

### netstat/ss
打印网络连接，路由表，接口统计信息，伪装连接和多播成员身份。

笔记：

- 大部分netstat信息适用于RoCE
- 对于开放的RDMA socket，请使用rdmatool（[**info**](http://man7.org/linux/man-pages/man8/rdma.8.html)）

### lldptool/dcbtool

管理lldpad（IEEE/CEE）的LLDP设置和状态。

处理lldptool-pfc（用于无损网络）（[**info**](https://linux.die.net/man/8/lldptool-pfc)）