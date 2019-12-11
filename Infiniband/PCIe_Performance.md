# 了解PCIe以获得最佳的性能
understanding-pcie-configuration-for-maximum-performance

拷贝自[mellanox](https://community.mellanox.com/s/article/understanding-pcie-configuration-for-maximum-performance)
## 为什么使用PCIe

PCIe用于任何系统中的不同模块之间的通信。网络适​​配器需要与CPU和内存（以及其他模块）进行通信。

这意味着为了处理网络流量，应正确配置通过PCIe进行通信的不同设备。将网络适配器连接到PCIe时，

它将自动协商网络适配器和CPU之间支持的最大功能。

## PCIe属性

任何PCI设备都加载了某些属性。其中一些属性对性能至关重要。通过在系统和设备功能之间进行协商来设置设备的PCIe属性。从而可以选择最高性能的选项。

在下面，您可以找到有关PCIe属性，如何验证它们以及它们对性能的影响的说明。

## PCIe宽度（Width）

PCIe宽度确定设备可以并行使用以进行通信的PCIe通道数。宽度标记为xA，其中A是通道数（例如8通道x8）。
Mellanox适配器支持x8和x16配置，具体取决于它们的类型。

为了验证PCIe宽度，可以使用命令**lspci**

在此示例中，我们在PCI 04.00.0地址上安装了Mellanox适配器。

    # lspci -s 04:00.0 -vvv | grep Width

    LnkCap: Port #0, Speed 8GT/s, Width x8, ASPM not supported, Exit Latency L0s unlimited, L1 unlimited

    LnkSta: Speed 8GT/s, Width x8, TrErr- Train- SlotClk+ DLActive- BWMgmt- ABWMgmt-

如您所见，PCIe报告了已通信的设备功能（在LnkCap下），以及它们的当前状态（在LnkSta下），这是实际的PCIe设备属性。

## PCIe速度（Speed）

确定可能的PCIe事务数。速度以GT/s表示，表示“每秒处理十的九次方数据”。

连同PCIe宽度一起， 确定最大PCIe带宽（速度*宽度）。

为了验证PCIe速度，可以使用命令**lspci**

    # lspci -s 04:00.0 -vvv | grep Speed

    LnkCap: Port #0, Speed 8GT/s, Width x8, ASPM not supported, Exit Latency L0s unlimited, L1 unlimited

    LnkSta: Speed 8GT/s, Width x8, TrErr- Train- SlotClk+ DLActive- BWMgmt- ABWMgmt-

与width参数类似，报告设备功能和状态。

PCIe速度被标识为“世代(版本)”，其中2.5GT/s被称为“ gen1”，5GT/s被称为“ gen2”，而8GT/s被称为“ gen3”。大多数Mellanox产品都支持所有世代。您也可以使用命令lspci查看PCIe的报告：

    # lspci -s 04:00.0 -vvv | grep PCIeGen

    [V0] Vendor specific: PCIeGen3 x8


注意：除了支持的速度外，各代之间的主要区别是数据包的编码开销。对于第1代和第2代，在PCIe上发送的每个数据包都有20％的PCIe标头开销。这在第3代中得到了改进，将开销降低到1.5％（2/130）。有关更多详细信息，请参见下面的实际PCIe带宽计算。

## PCIe最大有效负载大小

PCIe最大有效负载大小确定PCIe数据包或PCIe MTU（类似于网络协议）的最大大小。这意味着较大的PCIe事务被分解为PCIe MTU大小的数据包。此参数仅由系统设置，并取决于芯片组体系结构（例如x86_64，Power8，ARM等）。您可以使用命令lspci（在DevCtl下指定）查看PCIe最大有效负载大小。

    lspci -s 04:00.0 -vvv | grep DevCtl: -C 2

    DevCap: MaxPayload 512 bytes, PhantFunc 0, Latency L0s unlimited, L1 unlimited

    ExtTag+ AttnBtn- AttnInd- PwrInd- RBE+ FLReset+

    DevCtl: Report errors: Correctable- Non-Fatal+ Fatal+ Unsupported-

    RlxdOrd+ ExtTag+ PhantFunc- AuxPwr- NoSnoop+ FLReset-

    MaxPayload 256 bytes, MaxReadReq 4096 bytes

## PCIe最大读取请求

PCIe最大读取请求确定允许的最大PCIe读取请求。由于必须为传入响应准备缓冲区，PCIe设备通常会跟踪未决读取请求的数量。PCIe最大读取请求的大小可能会影响挂起请求的数量（当使用大于PCIe MTU的数据读取时）。再次，使用命令lspci来查询“最大读取请求”值：

    # lspci -s 04:00.0 -vvv | grep MaxReadReq

    MaxPayload 256 bytes, MaxReadReq 4096 bytes

与此处讨论的其他参数相反，可以在运行时使用命令setpci更改PCIe Max Read Request ：

首先，查询该值以避免覆盖其他属性

    # setpci -s 04:00.0 68.w

    5936

第一位数字是PCIe最大读取请求大小选择器。

设置选择器索引

    # setpci -s 04:00.0 68.w=2936

该值应使用命令**lspci**更新

    # lspci -s 04:00.0 -vvv | grep MaxReadReq

    MaxPayload 256 bytes, MaxReadReq 512 bytes

The acceptable values are: 0 - 128B, 1 - 256B, 2 - 512B, 3 - 1024B, 4 - 2048B and 5 - 4096B.

**注意：在此范围之外指定选择器索引可能会导致系统崩溃。**

## 计算PCIe限制

如前所述，PCIe功能可能会影响网络适配器的性能。很好地了解PCIe引入的带宽限制。以下是理论计算和一些示例。

最大可能的PCIe带宽是通过将PCIe宽度和速度相乘得出的。通过这个数字，我们为纠错协议和PCIe标头开销降低了〜1Gb/s。开销由PCIe编码（有关详细信息，请参阅PCIe速度）和PCIe MTU决定：

最大PCIe带宽=速度 * 宽度 *（1-编码）-1Gb / s。

例如，第3代PCIe设备的x8宽度将限于：

最大PCIe带宽= 8G * 8 *（1-2 / 130）-1G = 64G * 0.985-1G =〜62Gb / s。

另一个示例-具有x16宽度的第2代PCIe设备将限于：

最大PCIe带宽= 5G * 16 *（1-1 / 5）-1G = 80G * 0.8-1G =〜63Gb / s

注意： PCIe事务包括网络数据包有效负载和标头，因此在计算网络流量的PCIe限制时需要将它们考虑在内。

PCIe最大读取请求和最大有效负载大小可能会导致事务速率受到限制，这是因为在相同负载下PCIe总体和未决事务增加了。