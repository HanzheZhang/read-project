
# 拓扑结构是怎么形成的

交换机之间连接起来，就形成了一个稍微复杂的**拓扑结构**。

![两台交换机](https://static001.geekbang.org/resource/image/7a/73/7a40046c5a2c7f7cd3c95b54488b9773.jpg)

机器1发送ARP请求给机器4
    1. 机器1发起广播，机器2收到。
    2. 交换机A收到信息，转发给除了广播包发来的方向以外的其他网口
    3. 机器3收到广播
    4. 交换机B收到广播信息，将广播包转发给LAN3
    5. 机器4收到信息，发出反馈。

# 如何解决常见的环路问题
**环路**：当两个交换机将两个局域网同时连接起来

![环路](https://static001.geekbang.org/resource/image/c8/d2/c829b28978c3d9686680e4b62fdf53d2.jpg)

1. 机器1需要访问机器2
2. 交换机A此时还没有学习，并不知道A是在LAN1还是LAN2于是，广播到LAN2
3. LAN2存在着这个广播包，于是交换机B从右边接受到LAN2过来的广播包，广播到LAN1
4. 交换机A此时还是不知道机器2在哪，于是再次广播到LAN2
5. 不断重复3、4步骤

一开始交换机A和B都接受左边LAN1中机器1的广播包，学习到机器1是在左边的LAN1中。
但当交换机AB把该广播包广播到右边LAN2时，A接受到来自B的广播包，B也接受到来自A的广播包，不断循环。
其他机器也继续发广播包，通信链路越来越堵，最后走不动。
于是就产生了死循环的环路问题。

# STP协议中那些难以理解的概念

![](https://static001.geekbang.org/resource/image/5d/ba/5d3ba40babdacc735f617cc2356fefba.jpg)

STP协议的相关概念：
   - **ROOT Bridge**，也就是**根交换机**。
   - **Designated Bridge**，被翻译为**指定交换机**。
   - **Bridge Protocol Data Units（BPDU）**，**网桥协议数据单元**。
   - **Priority Vector**，**优先级向量**。

# STP的工作过程是怎样的
STP的工作过程：

1. 一开始，所有的交换机都认为自己是根交换机，每个网桥都被分配了一个ID。这个ID里有管理员分配的优先级，由此出现根交换机。
2. 所有的交换机互相发送BPDU。等级低的交换机会转发收到的BPDU，等级高的会继续发送BPDU。

![](https://static001.geekbang.org/resource/image/26/54/2666530a121ff1d1b079a330427efb54.jpg)

上图所示，数字表示优先级。在一个局域网中，两个交换机会比较谁的优先级比较高，确定从属关系。这种情况下局域网的合并会出现以下四种情形：

## 情形一：优先级高的遇见优先级高的

当5碰到了1，1和5都是各自局域网的高优先级交换机，所以两个交换机会进行比较找出优先级更高的。

## 情形二：同一个局域网中的交换机

当局域网形成“环”的状态就会产生此情形。

比如原本1-5-6是一个环，确定好最短通信路径，这个环通信就不成环了
- 5要是需要转发到1就，5->6->1
- 1要是需要转发到5就，1->6>5

## 情形三：等级高的和等级低的相遇

例如，2和7相遇，虽然7是等级低的，2是等级高的。但是7从属与1，1比2大，所以2要成为7的从属。

## 情形四：两个等级低的相遇

两个局域网的高等级交换机进行比较，所有的交换机将从属于等级最高的。

# 如何解决广播问题和安全问题

有两种划分方法：
   1. **物理隔离**
       
       使用单独的交换机，配置单独的子网，部门之间使用路由器沟通。
   2. **虚拟隔离**
       
       就是我们常说的**VLAN**，或者叫**虚拟局域网**。

在虚拟局域网中，需要在二层的头上加一个TAG，里面有一个VLAN ID，一共12位。12位可以划分4096个VLAN。

![](https://static001.geekbang.org/resource/image/2e/ed/2ede82f511ccac2570c17a62ffc749ed.jpg)

如果交换机是支持VLAN的，可以设置交换机每个端口所属的VLAN，交换机会把广播包的二层头取下来，识别VLAN ID，广播包的转发只在相同的端口 VLAN ID中进行。

交换机之间通过叫做Trunk口来进行连接，用来转发属于任何VLAN的口。 

![](https://static001.geekbang.org/resource/image/a1/4c/a134027334616274cf27f18841f7504c.jpg)

# 额外知识

1. **拓扑收敛慢**，当网络拓扑发生改变的时候，生成树协议需要50-52秒的时间才能完成拓扑收敛，数越大需要的时间越长，这期间就是网络中断。
2. **不能提供负载均衡的功能**。当网络中出现环路的时候，生成树协议简单的将环路进行Block，这样该链路就不能进行数据包的转发，浪费网络资源。
