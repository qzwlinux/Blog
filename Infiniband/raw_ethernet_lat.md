# raw_ethernet_lat

raw_ethernet_lat (raw Ethernet latency) tool is part of Perftest Package .

以太网延迟

    Server#raw_ethernet_lat
    Client#raw_ethernet_lat {Server_ip}

    # raw_ethernet_lat -h

    Usage:

    raw_ethernet_lat start a server and wait for connection

    raw_ethernet_lat <host> connect to server at <host>

    Options:

    -C, --report-cycles report times in cpu cycle units (default microseconds)

    -d, --ib-dev=<dev> Use IB device <dev> (default first device found)

    -D, --duration Run test for a customized period of seconds.

    -f, --margin measure results within margins. (default=2sec)

    -F, --CPU-freq Do not show a warning even if cpufreq_ondemand module is loaded, and cpu-freq is not on max.

    -g, --mcg Send messages to multicast group with 1 QP attached to it.

    -h, --help Show this help screen.

    -H, --report-histogram Print out all results (default print summary only)

    -i, --ib-port=<port> Use port <port> of IB device (default 1)

    -I, --inline_size=<size> Max size of message to be sent in inline

    -m, --mtu=<mtu> MTU size : 64 - 9600 (default port mtu)

    -M, --MGID=<multicast_gid> In multicast, uses <multicast_gid> as the group MGID.

    -n, --iters=<iters> Number of exchanges (at least 5, default 1000)

    -p, --port=<port> Listen on/connect to port <port> (default 18515)

    -r, --rx-depth=<dep> Rx queue size (default 512). If using srq, rx-depth controls max-wr size of the srq

    -s, --size=<size> Size of message to exchange (default 2)

    -S, --sl=<sl> SL (default 0)

    -T, --tos=<tos value> Set <tos_value> to RDMA-CM QPs. availible only with -R flag. values 0-256 (default off)

    -u, --qp-timeout=<timeout> QP timeout, timeout value is 4 usec * 2 ^(timeout), default 14

    -U, --report-unsorted (implies -H) print out unsorted results (default sorted)

    -V, --version Display version number

    --cpu_util Show CPU Utilization in report, valid only in Duration mode

    --dlid Set a Destination LID instead of getting it from the other side.

    --force-link=<value> Force the link(s) to a specific type: IB or Ethernet.

    --inline_recv=<size> Max size of message to be sent in inline receive

    --latency_gap=<delay_time> delay time between each post send

    --odp Use On Demand Paging instead of Memory Registration.

    --output=<units> Set verbosity output level: bandwidth , message_rate, latency

    Latency measurement is Average calculation

    --perform_warm_up Perform some iterations before start measuring in order to warming-up memory cache, valid in Atomic, Read and Write BW tests

    --pkey_index=<pkey index> PKey index to use for QP

    --tclass=<value> Set the Traffic Class in GRH (if GRH is in use)

    --use_exp Use Experimental verbs in data path. Default is OFF.

    --use_hugepages Use Hugepages instead of contig, memalign allocations.

    --use_res_domain Use shared resource domain

    --verb_type=<option> Set verb type: normal, accl. Default is normal.

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

    Example:

    Connect two servers back to back or via a switch.
    Make sure ping is running
    Run the following commands on the server and the client (replace the MAC addresses with your own):

    raw_ethernet_lat -d mlx5_1 -E ec:0d:9a:65:dc:e5 -B ec:0d:9a:c1:ba:61 --server
    raw_ethernet_lat -d mlx5_1 -B ec:0d:9a:65:dc:e5 -E ec:0d:9a:c1:ba:61 --client

    # raw_ethernet_lat -d mlx5_1 -E ec:0d:9a:65:dc:e5 -B ec:0d:9a:c1:ba:61 --server

    Min msg size for RawEth is 64B - changing msg size to 64

    ---------------------------------------------------------------------------------------

    Send Latency Test

    Dual-port : OFF Device : mlx5_1

    Number of qps : 1 Transport type : IB

    Connection type : RawEth Using SRQ : OFF

    TX depth : 1

    RX depth : 512

    Mtu : 1518[B]

    Link type : Ethernet

    GID index : 0

    Max inline data : 236[B]

    rdma_cm QPs : OFF

    Data ex. method : Ethernet

    ---------------------------------------------------------------------------------------

    MAC attached : EC:0D:9A:C1:BA:61

    **raw ethernet header****************************************

    --------------------------------------------------------------

    | Dest MAC | Src MAC | Packet Type |

    |------------------------------------------------------------|

    | EC:0D:9A:65:DC:E5| EC:0D:9A:C1:BA:61|DEFAULT |

    |------------------------------------------------------------|

    ---------------------------------------------------------------------------------------

    #bytes #iterations t_min[usec] t_max[usec] t_typical[usec] t_avg[usec] t_stdev[usec] 99% percentile[usec] 99.9% percentile[usec]

    64 1000 0.81 2.58 0.85 0.85 0.06 0.89 2.58

    ---------------------------------------------------------------------------------------