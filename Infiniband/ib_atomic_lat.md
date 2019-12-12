# ib_automic_lat

ib_atomic_lat (InfiniBand atomic latency) tool is part of Perftest Package 

IB原子延迟？

    Server#ib_atomic_lat

    # ib_atomic_lat -h

    Usage:

    ib_atomic_lat start a server and wait for connection

    ib_atomic_lat <host> connect to server at <host>

    Options:

    -A, --atomic_type=<type> type of atomic operation from {CMP_AND_SWAP,FETCH_AND_ADD} (default FETCH_AND_ADD)

    -c, --connection=<RC/XRC/DC> Connection type RC/XRC/DC (default RC)

    -C, --report-cycles report times in cpu cycle units (default microseconds)

    -d, --ib-dev=<dev> Use IB device <dev> (default first device found)

    -D, --duration Run test for a customized period of seconds.

    -e, --events Sleep on CQ events (default poll)

    -X, --vector=<completion vector> Set <completion vector> used for events

    -f, --margin measure results within margins. (default=2sec)

    -F, --CPU-freq Do not show a warning even if cpufreq_ondemand module is loaded, and cpu-freq is not on max.

    -h, --help Show this help screen.

    -H, --report-histogram Print out all results (default print summary only)

    -i, --ib-port=<port> Use port <port> of IB device (default 1)

    -m, --mtu=<mtu> MTU size : 256 - 4096 (default port mtu)

    -n, --iters=<iters> Number of exchanges (at least 5, default 1000)

    -o, --outs=<num> num of outstanding read/atom(default max of device)

    -p, --port=<port> Listen on/connect to port <port> (default 18515)

    -R, --rdma_cm Connect QPs with rdma_cm and run test on those QPs

    -S, --sl=<sl> SL (default 0)

    -T, --tos=<tos value> Set <tos_value> to RDMA-CM QPs. availible only with -R flag. values 0-256 (default off)

    -u, --qp-timeout=<timeout> QP timeout, timeout value is 4 usec * 2 ^(timeout), default 14

    -U, --report-unsorted (implies -H) print out unsorted results (default sorted)

    -V, --version Display version number

    -x, --gid-index=<index> Test uses GID with GID index (Default : IB - no gid . ETH - 0)

    -z, --com_rdma_cm Communicate with rdma_cm module to exchange data - use regular QPs

    --cpu_util Show CPU Utilization in report, valid only in Duration mode

    --dlid Set a Destination LID instead of getting it from the other side.

    --dont_xchg_versions Do not exchange versions and MTU with other side

    --force-link=<value> Force the link(s) to a specific type: IB or Ethernet.

    --inline_recv=<size> Max size of message to be sent in inline receive

    --ipv6 Use IPv6 GID. Default is IPv4

    --latency_gap=<delay_time> delay time between each post send

    --mmap=file Use an mmap'd file as the buffer for testing P2P transfers.

    --mmap-offset=<offset> Use an mmap'd file as the buffer for testing P2P transfers.

    --odp Use On Demand Paging instead of Memory Registration.

    --output=<units> Set verbosity output level: bandwidth , message_rate, latency


    Latency measurement is Average calculation

    --perform_warm_up Perform some iterations before start measuring in order to warming-up memory cache, valid in Atomic, Read and Write BW tests

    --pkey_index=<pkey index> PKey index to use for QP

    --retry_count=<value> Set retry count value in rdma_cm mode

    --tclass=<value> Set the Traffic Class in GRH (if GRH is in use)

    --use_exp Use Experimental verbs in data path. Default is OFF.

    --use_hugepages Use Hugepages instead of contig, memalign allocations.

    --use_res_domain Use shared resource domain

    --verb_type=<option> Set verb type: normal, accl. Default is normal.