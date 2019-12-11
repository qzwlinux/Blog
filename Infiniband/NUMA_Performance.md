# 了解NUMA节点

## 1.如何在PCI，设备，端口和NUMA之间映射？

运行“ mst status -v”是最简单方法。

这是一个服务器示例，该服务器安装了两张卡（ConnectX-4和ConnectX-3 Pro），每张卡都连接到不同的numa_node。

下面显示在PCI地址05：00.0上，mlx5_0是设备， 用于该设备的端口是ens785f0，而NUMA是0。


    # mst start
    ...

    # mst status -v

    MST modules:

    ------------

    MST PCI module loaded

    MST PCI configuration module loaded

    PCI devices:

    ------------

    DEVICE_TYPE MST PCI RDMA NET NUMA

    ConnectX4(rev:0) /dev/mst/mt4115_pciconf0.1 05:00.1 mlx5_1 net-ens785f1 0

    

    ConnectX4(rev:0) /dev/mst/mt4115_pciconf0 05:00.0 mlx5_0 net-ens785f0 0
    

    ConnectX3Pro(rev:0) /dev/mst/mt4103_pciconf0

    ConnectX3Pro(rev:0) /dev/mst/mt4103_pci_cr0 81:00.0 mlx4_0 net-ens817d1,net-ens817 1


## 2.如何将IB端口映射到CPU（numa_node）？

    # ibdev2netdev

    mlx4_0 port 1 ==> ens817 (Up)

    mlx4_0 port 2 ==> ens817d1 (Down)

    mlx5_0 port 1 ==> ens785f0 (Down)

    mlx5_1 port 1 ==> ens785f1 (Up)

    

    # cat /sys/class/net/ens785f0/device/numa_node

    0

    # cat /sys/class/net/ens817/device/numa_node

    1

## 3. 如何将PCI（根和功能）映射到numa_node？

    # lspci -D | grep Mellanox

    0000:05:00.0 Ethernet controller: Mellanox Technologies MT27700 Family [ConnectX-4]

    0000:05:00.1 Ethernet controller: Mellanox Technologies MT27700 Family [ConnectX-4]

    0000:81:00.0 Network controller: Mellanox Technologies MT27520 Family [ConnectX-3 Pro]

    

    # cat /sys/devices/pci0000\:00/0000\:00\:05.1/numa_node

    0

    # cat /sys/devices/pci0000\:00/0000\:00\:05.0/numa_node

    0

提示：在大多数情况下，如果适配器安装在以8开头的PCI地址（例如：81）中，则它将位于NUMA 1上。如果以0开头（例如：05），则它将位于NUMA 0中。 。

注意：如果系统不支持NUMA体系结构，则结果应为-1。

## 4.如何将CPU内核映射到NUMA节点？

每个CPU内核都映射到NUMA节点之一。在此示例中，通过获取CPU列表（cpulist），我们可以看到内核0-13和28-41映射到NUMA 0，而其余部分映射到NUMA 1。

    ＃cat /sys/devices/system/node/node0/cpulist

    0-13,28-41

    ＃cat /sys/devices/system/node/node1/cpulist

    14-27,42-55

该cpumap参数，位图提供了相同的结果。

    # cat /sys/devices/system/node/node0/cpumap

    000003ff,f0003fff <-- 0-13 & 28-41 bits are ON

    

    # cat /sys/devices/system/node/node1/cpumap

    00fffc00,0fffc000 <-- 14-27 & 42-55 bits are ON

# 在特定的NUMA节点上调用应用程序

## 1.如何在特定的NUMA节点上运行应用程序？
使用任务集应用程序，如下所示：

首先将ib_send_bw作为服务器运行以获取PID。

    ＃ib_write_bw＆

    [1] 45118

 接下来，获取核心关联。   

    # taskset -p 45118

    pid 45118's current affinity mask: ffffffffffffff


    # taskset -cp 0-13,28-41 45118

    pid 45118's current affinity list: 0-55

    pid 45118's new affinity list: 0-13,28-41

在此示例中，此任务可以在所有内核上运行。在我们的示例中，ConnectX-4连接到NUMA0。您可以更改相似性掩码以适合NUMA 0（0-13,28-41）使用的核心列表。  

    # taskset -p 45118

    pid 45118's current affinity mask: 3fff0003fff

在此示例中，使用-c标志在特定的NUMA内核上生成任务。

    # taskset -c 0-13,28-41 ib_send_bw &

    [1] 45292

    #

    ************************************

    * Waiting for client to connect... *

    ************************************

有关使用**taskset**的更多信息，请运行**taskset -h**，运行**man taskse**