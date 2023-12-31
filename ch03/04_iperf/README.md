
### 带宽与吞吐量

在数据传输的进程中，两个设备之间单位时间内数据流动的物理速度称为传输速率，单位为bps(Bits Per Second，每秒比特数)。所谓的百兆网卡、千兆网卡就是 100 Mbits/sec、1000 Mbits/sec 等。

以道路交通为例，低速数据链路就如同车道较少无法让很多车同时通过的情况。与之相反，高速数据链路就相当于有多个车道，一次允许更多车辆行驶的道路。传输速率又称作带宽(Bandwidth)。带宽越大网络传输能力就越强。

此外，主机之间实际的传输速率被称作吞吐量。其单位与带宽相同，都是bps(Bits Per Second)。

吞吐量这个词不仅衡量带宽，同时也衡量主机的 CPU 处理能力、网络的拥堵程度、报文中有效载荷的占有份额(不含报文首部，只计算数据字段本身)等信息。

测试带宽的方法有很多， iperf 是其中的一种。


### iperf 的使用

建立一个服务端:
```sh
    # iperf -s -p 12345 -i 1 -M
```

建立一个客户端:
```sh
    # iperf -c 192.168.2.102 -p 12345 -i 1 -t 20
```

服务端与客户端均位于同一台机器上时，由于进行了优化(流量不经过网卡，只是进程间通讯，不过该走的流程都走了)，并不是真正的带宽。
