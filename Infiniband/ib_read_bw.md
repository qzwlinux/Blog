# ib_read_bw

ib_read_bw (InfiniBand read bandwidth) tool is part of Perftest Package 

示例：

   Server#ib_read_bw
   Server#ib_read_bw {Server_ip} 

   # ib_read_bw -h

Usage:

ib_read_bw start a server and wait for connection

ib_read_bw <host> connect to server at <host>

Options:

-a, --all Run sizes from 2 till 2^23

-b, --bidirectional Measure bidirectional bandwidth (default unidirectional)

-c, --connection=<RC/XRC/DC> Connection type RC/XRC/DC (default RC)

-d, --ib-dev=<dev> Use IB device <dev> (default first device found)

-D, --duration Run test for a customized period of seconds.

-e, --events Sleep on CQ events (default poll)

-X, --vector=<completion vector> Set <completion vector> used for events

-f, --margin measure results within margins. (default=2sec)

-F, --CPU-freq Do not show a warning even if cpufreq_ondemand module is loaded, and cpu-freq is not on max.

-h, --help Show this help screen.

-i, --ib-port=<port> Use port <port> of IB device (default 1)

-l, --post_list=<list size> Post list of WQEs of <list size> size (instead of single post)

-m, --mtu=<mtu> MTU size : 256 - 4096 (default port mtu)

-n, --iters=<iters> Number of exchanges (at least 5, default 1000)

-N, --noPeak Cancel peak-bw calculation (default with peak up to iters=20000)

-o, --outs=<num> num of outstanding read/atom(default max of device)

-O, --dualport Run test in dual-port mode.

-p, --port=<port> Listen on/connect to port <port> (default 18515)

-q, --qp=<num of qp's> Num of qp's(default 1)

-Q, --cq-mod Generate Cqe only after <--cq-mod> completion

-R, --rdma_cm Connect QPs with rdma_cm and run test on those QPs

-s, --size=<size> Size of message to exchange (default 65536)

-S, --sl=<sl> SL (default 0)

-t, --tx-depth=<dep> Size of tx queue (default 128)

-T, --tos=<tos value> Set <tos_value> to RDMA-CM QPs. availible only with -R flag. values 0-256 (default off)

-u, --qp-timeout=<timeout> QP timeout, timeout value is 4 usec * 2 ^(timeout), default 14

-V, --version Display version number

-w, --limit_bw=<value> Set verifier limit for bandwidth

-x, --gid-index=<index> Test uses GID with GID index (Default : IB - no gid . ETH - 0)

-y, --limit_msgrate=<value> Set verifier limit for Msg Rate

-z, --com_rdma_cm Communicate with rdma_cm module to exchange data - use regular QPs

--cpu_util Show CPU Utilization in report, valid only in Duration mode

--dlid Set a Destination LID instead of getting it from the other side.

--dont_xchg_versions Do not exchange versions and MTU with other side

--force-link=<value> Force the link(s) to a specific type: IB or Ethernet.

--inline_recv=<size> Max size of message to be sent in inline receive

--ipv6 Use IPv6 GID. Default is IPv4

--mmap=file Use an mmap'd file as the buffer for testing P2P transfers.

--mmap-offset=<offset> Use an mmap'd file as the buffer for testing P2P transfers.

--mr_per_qp Create memory region for each qp.

--odp Use On Demand Paging instead of Memory Registration.

--output=<units> Set verbosity output level: bandwidth , message_rate, latency

Latency measurement is Average calculation

--perform_warm_up Perform some iterations before start measuring in order to warming-up memory cache, valid in Atomic, Read and Write BW tests

--pkey_index=<pkey index> PKey index to use for QP

--report-both Report RX & TX results separately on Bidirectinal BW tests

--report_gbits Report Max/Average BW of test in Gbit/sec (instead of MB/sec)

--report-per-port Report BW data on both ports when running Dualport and Duration mode

--reversed Reverse traffic direction - Server send to client

--run_infinitely Run test forever, print results every <duration> seconds

--retry_count=<value> Set retry count value in rdma_cm mode

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