# ibv_info

ibv_devinfo-查询RDMA设备

打印有关可从用户空间使用的RDMA设备的信息。

    -d，-- ib-dev = DEVICE
    使用IB设备DEVICE（找到默认的第一个设备）

    -i，-- ib-port = PORT 查询端口PORT（默认所有端口）

    -l，--list RDMA设备的唯一列表名称

    -v，-- verbose打印详细信息

    有关RDMA设备的信息



命令详细信息

    # ibv_devinfo -v
    hca_id: mlx4_0
            transport:                      InfiniBand (0)
            fw_ver:                         2.42.5000
            node_guid:                      0002:c903:001b:9f10
            sys_image_guid:                 0002:c903:001b:9f13
            vendor_id:                      0x02c9
            vendor_part_id:                 4099
            hw_ver:                         0x1
            board_id:                       MT_1100120019
            phys_port_cnt:                  1
            max_mr_size:                    0xffffffffffffffff
            page_size_cap:                  0xfffffe00
            max_qp:                         425912
            max_qp_wr:                      16351
            device_cap_flags:               0x057e9c76
                                            BAD_PKEY_CNTR
                                            BAD_QKEY_CNTR
                                            AUTO_PATH_MIG
                                            CHANGE_PHY_PORT
                                            UD_AV_PORT_ENFORCE
                                            PORT_ACTIVE_EVENT
                                            SYS_IMAGE_GUID
                                            RC_RNR_NAK_GEN
                                            XRC
                                            Unknown flags: 0x056e8000
            device_cap_exp_flags:           0x5000401600000000
                                            EXP_DEVICE_QPG
                                            EXP_UD_RSS
                                            EXP_CROSS_CHANNEL
                                            EXP_MR_ALLOCATE
                                            EXT_ATOMICS
                                            EXP_MASKED_ATOMICS
            max_sge:                        32
            max_sge_rd:                     30
            max_cq:                         65408
            max_cqe:                        4194303
            max_mr:                         524032
            max_pd:                         32764
            max_qp_rd_atom:                 16
            max_ee_rd_atom:                 0
            max_res_rd_atom:                6814592
            max_qp_init_rd_atom:            128
            max_ee_init_rd_atom:            0
            atomic_cap:                     ATOMIC_HCA (1)
            log atomic arg sizes (mask)             0x8
            masked_log_atomic_arg_sizes (mask)      0x8
            masked_log_atomic_arg_sizes_network_endianness (mask)   0x0
            max fetch and add bit boundary  64
            log max atomic inline           3
            max_ee:                         0
            max_rdd:                        0
            max_mw:                         0
            max_raw_ipv6_qp:                0
            max_raw_ethy_qp:                0
            max_mcast_grp:                  131072
            max_mcast_qp_attach:            244
            max_total_mcast_qp_attach:      31981568
            max_ah:                         2147483647
            max_fmr:                        0
            max_srq:                        65472
            max_srq_wr:                     16383
            max_srq_sge:                    31
            max_pkeys:                      128
            local_ca_ack_delay:             15
            hca_core_clock:                 427000
            max_klm_list_size:              0
            max_send_wqe_inline_klms:       0
            max_umr_recursion_depth:        0
            max_umr_stride_dimension:       0
            general_odp_caps:
            max_size:                       0x0
            rc_odp_caps:
                                            NO SUPPORT
            uc_odp_caps:
                                            NO SUPPORT
            ud_odp_caps:
                                            NO SUPPORT
            dc_odp_caps:
                                            NO SUPPORT
            xrc_odp_caps:
                                            NO SUPPORT
            raw_eth_odp_caps:
                                            NO SUPPORT
            max_dct:                        0
            max_device_ctx:                 1016
            Multi-Packet RQ is not supported
            rx_pad_end_addr_align:  0
            tso_caps:
            max_tso:                        0
            packet_pacing_caps:
            qp_rate_limit_min:              0kbps
            qp_rate_limit_max:              0kbps
            ooo_caps:
            ooo_rc_caps  = 0x0
            ooo_xrc_caps = 0x0
            ooo_dc_caps  = 0x0
            ooo_ud_caps  = 0x0
            sw_parsing_caps:
            supported_qp:
            tag matching not supported
            tunnel_offloads_caps:
            Device ports:
                    port:   1
                            state:                  PORT_ACTIVE (4)
                            max_mtu:                4096 (5)
                            active_mtu:             4096 (5)
                            sm_lid:                 3
                            port_lid:               2
                            port_lmc:               0x00
                            link_layer:             InfiniBand
                            max_msg_sz:             0x40000000
                            port_cap_flags:         0x02514868
                            max_vl_num:             8 (4)
                            bad_pkey_cntr:          0x0
                            qkey_viol_cntr:         0x0
                            sm_sl:                  0
                            pkey_tbl_len:           128
                            gid_tbl_len:            128
                            subnet_timeout:         18
                            init_type_reply:        0
                            active_width:           4X (2)
                            active_speed:           10.0 Gbps (8)
                            phys_state:             LINK_UP (5)
                            GID[  0]:               fe80:0000:0000:0000:0002:c903:001b:9f11
