# 了解如何检查CPU核心频率

要查看所有CPU内核是否以相同的频率运行并且每个内核是否以最大支持容量运行，请运行：

    # cat /sys/devices/system/cpu/cpu*/cpufreq/cpuinfo_max_freq

    3300000

    3300000

    3300000

    3300000

    3300000

    ...

查看当前CPU频率（用于比较）

    ＃cat /sys/devices/system/cpu/cpu*/cpufreq/cpuinfo_cur_freq

    1199975

    1200256

    1199975

    1200115

    1200677

    1199694

如果CPU频率未达到最大容量，请确保在BIOS中将设置设为最大性能。

请参阅了解[BIOS配置](https://community.mellanox.com/s/article/understanding-bios-configuration-for-performance-tuning)以了解[性能调整](https://community.mellanox.com/s/article/understanding-bios-configuration-for-performance-tuning)和[BIOS性能调整示例](https://community.mellanox.com/s/article/bios-performance-tuning-example)。有关更多说明。

