# raw_ethernet_bw

raw_ethernet_bw (raw Ethernet bandwidth) tool is part of Perftest Package

原始以太网带宽测试

    Server#raw_ethernet_bw
    Client#raw_ethernet_bw {Server_ip}

    # raw_ethernet_bw -h

    Usage:

    raw_ethernet_bw start a server and wait for connection

    raw_ethernet_bw <host> connect to server at <host>

    Options:

    -b, --bidirectional Measure bidirectional bandwidth (default unidirectional)

    -d, --ib-dev=<dev> Use IB device <dev> (default first device found)

    -D, --duration Run test for a customized period of seconds.

    -f, --margin measure results within margins. (default=2sec)

    -F, --CPU-freq Do not show a warning even if cpufreq_ondemand module is loaded, and cpu-freq is not on max.

    -g, --mcg Send messages to multicast group with 1 QP attached to it.

    -h, --help Show this help screen.

    -i, --ib-port=<port> Use port <port> of IB device (default 1)

    -I, --inline_size=<size> Max size of message to be sent in inline

    -l, --post_list=<list size> Post list of WQEs of <list size> size (instead of single post)

    -m, --mtu=<mtu> MTU size : 64 - 9600 (default port mtu)

    -M, --MGID=<multicast_gid> In multicast, uses <multicast_gid> as the group MGID.

    -n, --iters=<iters> Number of exchanges (at least 5, default 1000)

    -N, --noPeak Cancel peak-bw calculation (default with peak up to iters=20000)

    -p, --port=<port> Listen on/connect to port <port> (default 18515)

    -q, --qp=<num of qp's> Num of qp's(default 1)

    -Q, --cq-mod Generate Cqe only after <--cq-mod> completion

    -r, --rx-depth=<dep> Rx queue size (default 512). If using srq, rx-depth controls max-wr size of the srq

    -s, --size=<size> Size of message to exchange (default 65536)

    -S, --sl=<sl> SL (default 0)

    -t, --tx-depth=<dep> Size of tx queue (default 128)

    -T, --tos=<tos value> Set <tos_value> to RDMA-CM QPs. availible only with -R flag. values 0-256 (default off)

    -u, --qp-timeout=<timeout> QP timeout, timeout value is 4 usec * 2 ^(timeout), default 14

    -V, --version Display version number

    -w, --limit_bw=<value> Set verifier limit for bandwidth

    -y, --limit_msgrate=<value> Set verifier limit for Msg Rate

    --cpu_util Show CPU Utilization in report, valid only in Duration mode

    --dlid Set a Destination LID instead of getting it from the other side.

    --force-link=<value> Force the link(s) to a specific type: IB or Ethernet.

    --inline_recv=<size> Max size of message to be sent in inline receive

    --mr_per_qp Create memory region for each qp.

    --odp Use On Demand Paging instead of Memory Registration.

    --output=<units> Set verbosity output level: bandwidth , message_rate, latency

    Latency measurement is Average calculation

    --perform_warm_up Perform some iterations before start measuring in order to warming-up memory cache, valid in Atomic, Read and Write BW tests

    --pkey_index=<pkey index> PKey index to use for QP

    --report-both Report RX & TX results separately on Bidirectinal BW tests

    --report_gbits Report Max/Average BW of test in Gbit/sec (instead of MB/sec)

    --reversed Reverse traffic direction - Server send to client

    --run_infinitely Run test forever, print results every <duration> seconds

    --tclass=<value> Set the Traffic Class in GRH (if GRH is in use)

    --use_exp Use Experimental verbs in data path. Default is OFF.

    --use_hugepages Use Hugepages instead of contig, memalign allocations.

    --use_res_domain Use shared resource domain

    --verb_type=<option> Set verb type: normal, accl. Default is normal.

    --wait_destroy=<seconds> Wait <seconds> before destroying allocated resources (QP/CQ/PD/MR..)

    Rate Limiter:

    --burst_size=<size> Set the amount of messages to send in a burst when using rate limiter

    --rate_limit=<rate> Set the maximum rate of sent packages. default unit is [Gbps]. use --rate_units to change that.

    --rate_units=<units> [Mgp] Set the units for rate limit to MBps (M), Gbps (g) or pps (p). default is Gbps (g).

    Note (1): pps not supported with HW limit.

    Note (2): When using PP rate_units is forced to Kbps.

    --rate_limit_type=<type> [HW/SW/PP] Limit the QP's by HW, PP or by SW. Disabled by default. When rate_limit Not is specified HW limit is Default.

    Note (1) in Latency under load test SW rate limit is forced

    Raw Ethernet options :

    -B, --source_mac source MAC address by this format XX:XX:XX:XX:XX:XX **MUST** be entered

    -E, --dest_mac destination MAC address by this format XX:XX:XX:XX:XX:XX **MUST** be entered

    -G, --use_rss use RSS on server side. need to open 2^x qps (using -q flag. default is -q 2). open 2^x clients that transmit to this server

    -J, --dest_ip destination ip address by this format X.X.X.X for IPv4 or X:X:X:X:X:X for IPv6 (using to send packets with IP header)

    -j, --source_ip source ip address by this format X.X.X.X for IPv4 or X:X:X:X:X:X for IPv6 (using to send packets with IP header)

    -K, --dest_port destination port number (using to send packets with UDP header as default, or you can use --tcp flag to send TCP Header)

    -k, --source_port source port number (using to send packets with UDP header as default, or you can use --tcp flag to send TCP Header)

    -Y, --ethertype ethertype value in the ethernet frame by this format 0xXXXX

    -Z, --server choose server side for the current machine (--server/--client must be selected )

    -P, --client choose client side for the current machine (--server/--client must be selected)

    -v, --mac_fwd run mac forwarding test

    --disable_fcs Disable Scatter FCS feature. (Scatter FCS is enabled by default when using --use_exp flag).

    --flows set number of TCP/UDP flows, starting from <src_port, dst_port>.

    --flows_burst set number of burst size per TCP/UDP flow.

    --promiscuous run promiscuous mode.

    --reply_every in latency test, receiver pong after number of received pings

    --sniffer run sniffer mode.

    --tcp send TCP Packets. must include IP and Ports information.

    --raw_ipv6 send IPv6 Packets.

    --flow_label IPv6 flow label