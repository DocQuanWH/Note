# TCP & UDP

## 1. TCP和UDP的区别

| **功能** | **TCP** | **UDP** |
| --- | --- | --- |
| 是否建立连接 | 需建立连接 | 无需建立连接 |
| 是否可靠 | 提供可靠传输：有序、无差错、无丢失、不重复 | 不可靠 |
| 是否提供网络控制 | 提供流量控制和拥塞控制机制 | 不提供流量控制和拥塞控制机制 |
| 数据格式 | 字节流：需要先将**数据分组**，然后再接收端**重组** | 数据报 |

## 2. TCP建立连接
TCP通过三次握手建立可靠连接：
* 客户端（SYN-SENT）->服务器发起**建立连接请求**：`[SYN], seq = x`
* 服务器（SYN-RCVD）->客户端返回**确认信息**：`[SYN, ACK], seq = y, ack = x + 1`
* 客户端（ESTABLISHED）->服务器返回**确认信息**：`[ACK], seq = x + 1, ack = y + 1`。服务器在接收到确认信息后，状态变为**ESTABLISHED**

TCP需要**三次握手**建立连接原因：**为了防止失效的（网络延迟）客户端连接请求包被服务器接收**，因此必须通过客户端的第三次确认才建立连接。

## 3. TCP终止连接
TCP连接建立完成后，通信双方都可以发起链接终止请求，为简单起见，将终止发起方叫做客户端。TCP通过**四次挥手**中断连接：
* 客户端（FIN-WAIT-1）->服务器发起**中断连接请求**：`[FIN], seq = x`
* 服务器（CLOSE-WAIT）->客户端返回**确认信息**：`[ACK], seq = y, ack = x + 1`
* 服务器（LAST-ACK）->客户端（FIN-WAIT-2）发起**中断连接请求**：`[FIN, ACK], seq = y + 1, ack = x + 1`
* 客户端（TIME-WAIT）->服务器返回确认信息：`[ACK], seq = x + 1, ack = y + 1`

TCP需要**四次挥手**中断连接原因：
* **主要是为了确保数据能够完成传输**，因为TCP是全双工，客户端请求关闭，但可能服务器还需要发送数据给客户端，数据发送完毕后服务器再请求关闭，因此两方都需要发送FIN包关闭连接。
* **TIME-WAIT**是为了保证客户端的确认信息被服务端收到，否则将等待服务器重新发送终止请求（服务端在没收到确认信息一定时间后会再次发送终止请求）。

## 4. TCP滑动窗口
TCP通过**大小可变的滑动窗口**来管理数据的发送和接收，客户端每发送一个包，都需要经过服务端确认。通过滑动窗口可以进行流量控制和拥塞控制：
* **流量控制**：根据接收端接收能力（缓冲区大小）来控制发送端滑动窗口的大小。其目的是“让发送方的发送速率不要太快，要让接收方来得及接收”。
* **拥塞控制**：将网络中的TCP包数量维持在一定的数量之下，当超过该数值时，网络的性能会急剧恶化。通过**慢开始**、**拥塞避免**、**快重传**和**快恢复**四种算法进行拥塞控制。

## 5. TCP重传机制
TCP每发送一个报文段，就设置一次**定时器**（根据网络状况动态改变的定时器），只要定时器设置的重发时间到而还没有收到确认，就要重发这一报文段。
