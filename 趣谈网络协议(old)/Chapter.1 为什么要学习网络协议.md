
# Chapter1 为什么要学习网络协议
## 协议三要素

1. 语法:
就是一段内容要符合一定的规则和格式。
2. 语义:
就是这一段内容要代表的某种意义
3. 顺序:
就是协议操作的顺序

## 常见的网络协议

1. HTTP协议:POST + URL + HTTP1.1 + 正文格式：json + 正文长度：1234
    
DNS, HTTP, HTTPS所在的网络层我们称之为应用层。经过应用层封装之后，浏览器将会把**应用层**的包交给下一层去完成，通过socket编程来实现。

2. TCP协议

应用层的下一层为**传输层**。传输层有两种协议，一种是无连接的协议**UDP**,另一种是面向链接的协议**TCP**。
所谓面向链接，就是TCP会保证这个包能够达到目的地。如果不能达到，就会重新发送，直至到达。
TCP协议里面有两个端口，一个是浏览器监听的端口，另一个是服务器监听的端口。操作系统往往通过端口来判断包应该交给哪个进程。

3. IP协议

传输层封装完毕后，浏览器会将包交给操作系统的**网络层**。网络层使用IP协议。
IP协议里面会有源IP地址，即浏览器所在机器的IP地址和目标IP地址。操作系统通过源IP地址就可以将包发送给目标。

4. DHCP协议

操作系统通过源IP将包发送给目标，就要通过**网关**。
操作系统收到源IP地址的时候，就会被DHCP协议配置IP地址，以及默认的网关地址192.168.1.1

5. ARP协议

经过DHCP配置之后，操作系统会使用ARP协议用来寻找对应的网关，当IP发出后，对应的网关就会发来一个网关的本地地址**MAC地址**。本地网络就会将包交给下一层**MAC层**。

6 路由协议

常用的路由协议有**OSPF**和**BGP**。
包使用路由协议在外部网络中传输。

![网络通信协议](https://images0.cnblogs.com/blog/471885/201405/181609190626340.jpg)
