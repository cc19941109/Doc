# TCP/IP

## 参考

计算机网络 - 谢希仁

## 协议层

- 物理层
- 数据链路层
- 网络层
- 传输层
- 应用层

## 传输层

### 传输层和网络层

- 网路层是为主机之间提供逻辑通信
- 运输层为应用进程之间提供端对端的逻辑通信

### 功能特性

#### 复用  multiplex

发送方不同的应用进程都可以使用一个传输层协议传送数据.

#### 分用 demultiplex

接受方的传输层,在剥去报文的首部后,能把这些数据正确的交付目的应用进程.

#### 软件端口

协议栈层间的抽象的协议端口就是软件端口

应用层的各种协议进程与运输实体进行层间交互的一种地址

16位端口号 -- 共 65535 个

### UDP

首部共8个字节

- 2个字节  源端口
- 2个字节 目的端口
- 2个字节长度 长度
- 2个字节  检验和


### TCP

首部共20字节














