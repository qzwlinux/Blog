# Kubernetes源码安装

[TOC]


## 前期准备

### 结构图

![](index_files/16bbd125-a76e-4ddb-9401-3ae821b53653.jpg)
### 使用root账号登录
sudo比较麻烦

### 关闭系统交换分区功能
    swapoff -a
编辑文件`/etc/fstab`
![](index_files/0e71272c-f60c-4bb3-852e-f426a3a0c649.png)
注释掉swap分区后重启

## Master节点安装

Master节点中需要安装的有5个模块：
- etcd
- kube-apiserver
- kube-controller-manager
- kube-scheduler


** Kubernetes服务**
项目文件地址：（https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.10.md#v11012）

    wget 'https://dl.k8s.io/v1.10.12/kubernetes-server-linux-amd64.tar.gz'
    sudo tar -zxvf kubernetes-server-linux-amd64.tar.gz -C /usr/local/src/
    cd /usr/local/src/kubernetes/server/bin
    sudo cp kube-apiserver kube-controller-manager kube-scheduler /usr/bin/

**网络管理服务ifupdown**

    wget https://mirrors.aliyun.com/ubuntu/pool/main/i/ifupdown/ifupdown_0.8.17ubuntu1.1_amd64.deb
    dpkg -i ifupdown_0.8.17ubuntu1.1_amd64.deb
### etcd服务

etcd是Kubernetes集群的主数据库，安装Kubernetes服务前需要安装并启动。
从Github中下载etcd二进制包（https://github.com/etcd-io/etcd/releases/download/v3.3.10/etcd-v3.3.10-linux-amd64.tar.gz）

**配置etcd服务**

    wget 'https://github.com/etcd-io/etcd/releases/download/v3.3.10/etcd-v3.3.10-linux-amd64.tar.gz'
    sudo tar -zxvf etcd-v3.3.10-linux-amd64.tar.gz -C /usr/local/src/
    sudo cp /usr/local/src/etcd-v3.3.10-linux-amd64/etc* /usr/bin
    

-------------------------------------------
编辑启动文件 `/usr/lib/systemd/system/etcd.service`

    [Unit]
    Description=Etcd Server
    After=network-manager.target
    [Service]
    Type=notify
    EnvironmentFile=-/etc/etcd/etcd.conf
    WorkingDirectory=/var/lib/etcd/
    ExecStart=/usr/bin/etcd
    Restart=on-failure
    [Install]
    WantedBy=multi-user.target

**启动etcd服务**

    systemctl daemon-reload
    echo "先添加目录后启动服务"
    mkdir -p /var/lib/etcd/
    systemctl start etcd.service
    systemctl enable etcd.service



**测试etcd服务是否正常工作**

    netstat -anop | grep etcd
    etcdctl cluster-health


### kube-apiserver
kube-apiserver是apiserver的主程序

编辑启动文件 `/usr/lib/systemd/system/kube-apiserver.service
`

    [Unit]
    Description=Kubernetes API Server
    Documentation=https://github.com/kubernetes/kubernetes
    After=etcd.service
    Wants=etcd.service
    [Service]
    EnvironmentFile=/etc/kubernetes/apiserver.conf
    ExecStart=/usr/bin/kube-apiserver $KUBE_API_ARGS
    Restart=on-failure
    Type=notify
    [Install]
    WantedBy=multi-user.target
--------------------------------------------------
新建配置文件目录

    mkdir -p /etc/kubernetes

编辑配置文件  `vim /etc/kubernetes/apiserver.conf`

    KUBE_API_ARGS="--storage-backend=etcd3 --etcd-servers=http://127.0.0.1:2379 --insecure-bind-address=0.0.0.0 --insecure-port=8080 --service-cluster-ip-range=10.10.10.0/24 --service-node-port-range=1-65535 --admission-control=NamespaceLifecycle,NamespaceExists,LimitRanger,SecurityContextDeny,ServiceAccount,DefaultStorageClass,ResourceQuota --logtostderr=true --log-dir=/var/log/kubernetes --v=2"


*启动项说明*
- `--storage-backend`指定etcd版本
- `--etcd-server`指定etcd服务对应的端口
- `--insecure-bind-addres`apiserver绑定主机的非安全ip地址
- `--insecure-port`apiserver绑定主机非安全的端口号
- `--service-cluster-ip-rang`k8s集群中service虚拟ip地址端范围，以CIDR格式表示，且不可与物理机现有IP段重合
- `--service-node-port-range`k8s集群中service可映射物理机端口号范围，默认30000~32767
- ` --admission-control`k8s集群准入控制设置
- `--logtostderr`设置写入日志到文件
- `--log-dir`日志存放目录
- `--v`日志级别

### kube-controller-manager
是集群内部的管理控制中心、负责k8s集群发现并修复，确保集群处于预期工作状态

编辑启动文件`/usr/lib/systemd/system/kube-controller-manager.service`
    [Unit]
    Description=Kubernetes Controller Manager
    Documentation=https://github.com/GoogleCloudPlatform/kubernetes
    After=kube-apiserver.service
    Requires=kube-apiserver.service

    [Service]
    EnvironmentFile=-/etc/kubernetes/controller-manager.conf
    ExecStart=/usr/bin/kube-controller-manager $KUBE_CONTROLLER_MANAGER_ARGS
    Restart=on-failure
    LimitNOFILE=65536

    [Install]
    WantedBy=multi-user.target

编辑配置文件`/etc/kubernetes/controller-manager.conf`

    KUBE_CONTROLLER_MANAGER_ARGS="--master=http://192.168.124.130:8080 --logtostderr=true --log-dir=/var/log/kubernetes --v=2"

- `--master`指定apiserver的端口地址

### kube-scheduler

kubernetes scheduler的任务就是将pod调度到最合适的Node

编辑启动文件 `/usr/lib/systemd/system/kube-scheduler.service`

    [Unit]
    Description=Kubernetes Scheduler
    Documentation=https://github.com/GoogleCloudPlatform/kubernetes
    After=kube-apiserver.service
    Requires=kube-apiserver.service

    [Service]
    EnvironmentFile=-/etc/kubernetes/scheduler.conf
    ExecStart=/usr/bin/kube-scheduler $KUBE_SCHEDULER_ARGS
    Restart=on-failure
    LimitNOFILE=65536

    [Install]
    WantedBy=multi-user.target

------------------------------------------------------------------------------------------------------------------------------
编辑配置文件 `/etc/kubernetes/scheduler.conf`

    KUBE_SCHEDULER_ARGS="--master=http://192.168.124.130:8080 --logtostderr=true --log-dir=/var/log/kubernetes --v=2"    

### 配置完成启动
    systemctl daemon-reload
    systemctl enable kube-apiserver.service
    systemctl start kube-apiserver.service
    systemctl enable kube-controller-manager.service
    systemctl start kube-controller-manager.service
    systemctl enable kube-scheduler.service
    systemctl start kube-scheduler.service

*检查每个服务的健康状态：*

    systemctl status kube-apiserver.service
    systemctl status kube-controller-manager.service
    systemctl status kube-scheduler.service
如果有服务出现异常，可执行`journalctl -xe -u kube-scheduler`查看一个指定的服务的事件信息日志。

## Node节点安装

Node节点中需要安装两个kubernetes服务
- kubelet
- kube-proxy

** Kubernetes服务**
项目文件地址：（https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG-1.10.md#v11012）

    wget 'https://dl.k8s.io/v1.10.12/kubernetes-server-linux-amd64.tar.gz'
    sudo tar -zxvf kubernetes-server-linux-amd64.tar.gz -C /usr/local/src/
    cd /usr/local/src/kubernetes/server/bin
    sudo cp kubelet kube-proxy /usr/bin/

### 安装docker
为了保证项目可以在主机不连接互联网的情况下进行，安装使用下载好的deb包进行安装。

#### 下载所需要的安装包
- docker-ce
- docker-ce-cli
- containerd.io
- libltdl7
- docker-compose
- ifupdown
--------------------------------------------
    wget https://download.docker.com/linux/ubuntu/dists/bionic/pool/stable/amd64/docker-ce_18.09.0~3-0~ubuntu-bionic_amd64.deb
    wget https://download.docker.com/linux/ubuntu/dists/bionic/pool/stable/amd64/containerd.io_1.2.0-1_amd64.deb
    wget https://download.docker.com/linux/ubuntu/dists/bionic/pool/stable/amd64/docker-ce_18.09.0~3-0~ubuntu-bionic_amd64.deb
    wget https://mirrors.aliyun.com/ubuntu/pool/main/libt/libtool/libltdl7_2.4.6-2_amd64.deb
    wget https://github.com/docker/compose/releases/download/1.23.2/docker-compose-Linux-x86_64

**网络管理服务ifupdown**

    wget https://mirrors.aliyun.com/ubuntu/pool/main/i/ifupdown/ifupdown_0.8.17ubuntu1.1_amd64.deb
    dpkg -i ifupdown_0.8.17ubuntu1.1_amd64.deb
----------------------------------------------------

#### 安装docker包

    dpkg -i containerd.io_1.2.0-1_amd64.deb libltdl7_2.4.6-2_amd64.deb docker-ce_18.09.0~3-0~ubuntu-bionic_amd64.deb docker-ce-cli_18.09.0~3-0~ubuntu-bionic_amd64.deb

*运行docker version查看是否安装成功*

    docker version
*设置docker自启动*

    systemctl enable docker
#### docker-compose安装

    mv docker-compose-Linux-x86_64 /usr/bin/docker-compose
    chmod +x /usr/bin/docker-compose
*查看docker-compose状态*

    docker-compose version

### kubelet服务

Kubelet组件运行在Node节点上，维持运行中的Pods以及提供Kuberntes运行时环境，主要完成以下使命：
- 监视分配给该Node节点的pods
- 挂载pod所需要的volumes
- 下载pod的secret
- 通过docker/rkt来运行pod中的容器 
- 周期的执行pod中为容器定义的liveness探针
- 上报pod的状态给系统的其他组件
- 上报Node的状态

编辑启动文件`/usr/lib/systemd/system/kubelet.service`

    [Unit]
    Description=Kubernetes Kubelet Server
    Documentation=https://github.com/GoogleCloudPlatform/kubernetes
    After=docker.service
    Requires=docker.service

    [Service]
    WorkingDirectory=/var/lib/kubelet
    EnvironmentFile=-/etc/kubernetes/kubelet.conf
    ExecStart=/usr/bin/kubelet $KUBELET_ARGS
    Restart=on-failure
    KillMode=process

    [Install]
    WantedBy=multi-user.target

编辑配置文件`/etc/kubernetes/kubelet.conf`

    KUBELET_ARGS="--address=192.168.124.131 --port=10250 --hostname-override=192.168.124.131 --allow-privileged=false --kubeconfig=/etc/kubernetes/kubelet.kubeconfig --cluster-dns=10.10.10.2 --cluster-domain=cluster.local --fail-swap-on=false --logtostderr=true --log-dir=/var/log/kubernetes --v=4"

编辑连接apiserver的文件`/etc/kubernetes/kubelet.kubeconfig`

    apiVersion: v1
    kind: Config
    clusters:
      - cluster:
          server: http://192.168.124.130:8080
        name: local
    contexts:
      - context:
          cluster: local
        name: local
    current-context: local


启动kubelet服务：

    mkdir -p /var/lib/kubelet
    systemctl daemon-reload
    systemctl enable kubelet
    systemctl start kubelet
    systemctl status kubelet

### kube-proxy服务
kube-proxy是一个负载均衡程序，接收外部客户端发来的请求将其转发给Pod

编辑启动文件`/usr/lib/systemd/system/kube-proxy.service`

    [Unit]
    Description=Kubernetes Kube-proxy Server
    Documentation=https://github.com/GoogleCloudPlatform/kubernetes
    After=networking.service
    Requires=networking.service

    [Service]
    EnvironmentFile=/etc/kubernetes/proxy.conf
    ExecStart=/usr/bin/kube-proxy $KUBE_PROXY_ARGS
    Restart=on-failure
    LimitNOFILE=65536
    KillMode=process

    [Install]
    WantedBy=multi-user.target

编辑配置文件`/etc/kubernetes/proxy.conf`

    KUBE_PROXY_ARGS="--master=http://192.168.124.130:8080 --hostname-override=10.0.2.6 --logtostderr=true --log-dir=/var/log/kubernetes --v=4"

启动kube-proxy服务：

    systemctl daemon-reload
    systemctl enable kube-proxy
    systemctl restart kube-proxy
    systemctl status kube-proxy
