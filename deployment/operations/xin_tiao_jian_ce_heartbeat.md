# 网络心跳配置

aerospike的心跳协议负责维护集群的完整性。有两种方式心跳模式

* Multicast (UDP)
* Mesh (TCP)


## 多播 Multicast Heartbeat
我们建议使用多播心跳协议时可用。由于各种原因,你的网络可能不支持多播。请查看我们的[故障排除指南](http://www.aerospike.com/docs/operations/troubleshoot/)，以查看如何在您的环境中验证多播信息。


###配置步骤

在 heartbeat 段落里面配置：

* Set mode to multicast.

* Set address to a valid multicast address (239.0.0.0-239.255.255.255).

*(Optional) Set interface-address to the IP of the interface intended for intracluster communication. This setting also controls the interface fabric will use. Needed when isolating intra-cluster traffic to a particular network interface. InterfaceAddress 表示一个由名称和分配给此接口的 IP 地址列表组成的网络接口。它用于标识加入多播组的本地接口。
  

* Set interval and timeout

* interval (recommended: 150) controls how often to send a heartbeat packet.

* timeout (recommended: 10) controls the number of intervals after which a node is

* considered to be missing by rest of nodes in the cluster if they haven't received the heartbeat from missing node.

* With the default settings, a node will be aware of another node leaving the cluster within 1.5 seconds.

##Example

```java
...
  heartbeat {
    mode multicast                  # Send heartbeats using Multicast
    address 239.1.99.2              # multicast address
    port 9918                       # multicast port
    interface-address 192.168.1.100 # (Optional) (Default any) IP of the NIC to
                                    # use to send out heartbeat and bind
                                    # fabric ports
    interval 150                    # Number of milliseconds between heartbeats
    timeout 10                      # Number of heartbeat intervals to wait
                                    # before timing out a node
  }
...
```

## 单播 Mesh (Unicast) Heartbeat

采用TCP点对点连接的心跳。在集群中的每个节点维护一个心跳连接到所有其他节点，导致多播所需的许多连接。基于这个原因，我们建议使用多播心跳协议时可用。

###配置步骤

* Set mode to mesh.

* (Optional) Set address to the IP of the local interface intended for intracluster communication. This setting also controls the interface fabric will use. Needed when isolating intra-cluster traffic to a particular network interface.

* Set mesh-seed-address-port to be the IP address and heartbeat port of a node in the cluster.

* Set interval and timeout

* interval (recommended: 150) controls how often to send a heartbeat packet.

* timeout (recommended: 10) controls the number of intervals after which a node is considered to be missing by the rest of the nodes in the cluster if they haven't received the heartbeat from the missing node.

* With the recommended settings, a node will be aware of another node leaving the cluster within 1.5 seconds.


````java
...
  heartbeat {
    mode mesh                   # Send heartbeats using Mesh (Unicast) protocol
    address 192.168.1.100       # (Optional) (Default: any) IP of the NIC on
                                # which this node is listening to heartbeat
    port 3002                   # port on which this node is listening to
                                # heartbeat
    mesh-seed-address-port 192.168.1.100 3002 # IP address for seed node in the cluster
                                              # This IP happens to be the local node
    mesh-seed-address-port 192.168.1.101 3002 # IP address for seed node in the cluster
    mesh-seed-address-port 192.168.1.102 3002 # IP address for seed node in the cluster
    mesh-seed-address-port 192.168.1.103 3002 # IP address for seed node in the cluster

    interval 150                # Number of milliseconds between heartbeats
    timeout 10                  # Number of heartbeat intervals to wait before
                                # timing out a node
  }
...```

