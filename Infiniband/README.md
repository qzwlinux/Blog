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

