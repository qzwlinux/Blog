# 如何将cpu缩扩展调节器设置为最大性能

这是修改CPU性能的配置指南。

**scaling_governor**功能可为CPU设置静态频率。

频率值必须介于scale_min_freq 和scale_max_freq之间。

当CPU频率调节器设置为“节能”模式时，CPU设置为最低静态频率（在scale_min_freq和scale_max_freq的边界内）。

为了获得最佳性能，建议将CPU频率调节器“ scaling_governor ”设置为“性能”模式。

## 配置

1. 要查看每个CPU的当前**scaling_governor**值，请运行：

        # cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor

        powersave

        powersave

        powersave

        powersave

        powersave

        ...

2. 要将每个CPU的**scaling_governor**设置为“性能”模式，请运行：

        # echo performance > /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor

        # echo performance > /sys/devices/system/cpu/cpu1/cpufreq/scaling_governor

        # echo performance > /sys/devices/system/cpu/cpu2/cpufreq/scaling_governor

        # echo performance > /sys/devices/system/cpu/cpu3/cpufreq/scaling_governor

        ...

3. 要验证配置，请运行：

        # cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor

        performance
        performance
        performance
        performance
        ...

4. 要查看和比较当前频率与最小和最大比例，请运行：

        # cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_max_freq

        3300000

        # cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_min_freq

        1200000

        # cat /sys/devices/system/cpu/cpu0/cpufreq/cpuinfo_cur_freq

        3210156