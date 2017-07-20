## 一些问题

### 1. 字节顺序
字节存储机制主要有两种：
* **大端模式**：低位存放在**高地址**中
* **小端模式**：低位存放在**低地址**中

比如十六进制数字`0x01020304`存放方式：

|类型|0x00ff3304|0x00ff3303|0x00ff3302|0x00ff3301|
|---|---|---|---|---|
|大端|04（低位）|03|02|01|
|小端|01|02|03|04（低位）|

**网络字节顺序（大端）**：按高位到低位顺序存储

**主机字节顺序**：由具体主机确定