# 网络
### 1. 网络模型
网络间的数据通信遵循一系列协议才能进行，实际上可以把网络看做是一系列网络协议。
#### 1.1. OSI七层参考模型
|应用层|表示层|会话层|传输层|网络层|数据链路层|物理层|
|---|---|---|---|---|---|---|
#### 1.2. TCP/IP四层模型
|TCP/IP四层模型|协议族|
|:---|:---|
|应用层|HTTP、FTP、SMTP、IMAP、DNS、DHCP...|
|传输层|TCP、UDP...|
|网络层|IP、ICMP、ARP、RARP|
|网络接口层：数据链路层&物理层|Ethernet、ISDN...|
#### 1.3. 数据封装过程（以太网帧）
|以太网首部|IP首部|TCP首部|*应用数据*|以太网尾部|
|---|---|---|---|---|
### 2. 物理层（Physical Layer）
网络模型的最底层，负责传输01信号，构成网络的基本：光缆、电缆、双绞线...
### 3. 链路层（Link Layer）
单纯的01信号没有太大意义，需要通过链路层将01信号组成一组，表示某一种数据的分段
#### 3.1 以太网协议
一组信号构成一个数据帧，每一帧分为两个部分：标头（18bytes）和数据（46~1500bytes）。因此，一帧的数据大小范围为[64,1518]，如果数据太长，就必须切割数据。
#### 3.2 MAC地址
以太网标头包含接收者和发送者信息，这些信息就是网络设备**唯一**的MAC地址（48bit，12个十六进制数表示）。
#### 3.3 广播
一块网卡通过ARP协议可以获取另一块网卡的MAC地址，然后再通过广播的方式向所有本网络的设备发送数据，然后由接收方自行判断数据是否该自己接收。
### 4. 网络层
通过IP+MAC地址，网络层可以确定主机到主机的通信。理论上说MAC地址就可以实现数据的发送，但如果通过MAC向整个网络广播数据，肯定会引起数据灾难，此时通过IP地址来区分不同的网络，MAC区分终端。通过**子网掩码**可以确定两个IP是否处于同一个子网，如255.255.255.0与IP做and运算，如果计算结果相同，表示两个IP处于同一个子网。
### 5. 传输层
通过端口/套接字（Socket，0~65535），传输层可以确定进程到进程之间的通信。在数据包中添加端口信息，就需要添加新的协议去约束数据的传输：TCP和UDP。
#### 5.1 UDP
UDP数据包标头定义接收和发送端端口号，标头8bytes，总长度不超过65535bytes。
#### 5.2 TCP
TCP标头和UDP一样，但数据包可以理论上无限长，不超过IP包的长度，确保数据包不会被分割（分割需要时间）。
#### 5.3 TCP的三次握手
TCP有以下几种标识位：
* SYN：建立连接
* ACK：确认
* PSH：push传送
* FIN：结束
* RST：重置
* URG：紧急

TCP通过三次握手建立可靠连接：
1. 客户端向服务器发起连接请求：`SYN = m`，客户端的**SYN**状态为**\_SENT**
2. 服务器向客户端返回确认信息：`ACK = m + 1`（表示服务器接收到连接请求），`SYN = n`，服务器

### 6. 应用层
互联网是开放架构，数据格式五花八门，因此需要通过应用层来规定应用程序的数据格式。常用的协议有：HTTP、FTP、SMTP/IMAP/POP...
