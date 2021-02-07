---
title: CSharp-基于TCP的Socket通信
date: 2018-11-30 20:39:42
tags: 
    - CSharp
    - TCP
top:
img: https://cdn.jsdelivr.net/gh/TonyChenn/BlogPicture/2018/11.30/tcp.png
description:
---
# TCP三次握手
在最开始客户端(Client)和服务端(Server)都处于`CLOSED`状态。

Server首先创建传输控制块`TCB`,等待客户端连接，进入监听`LISTEN`状态。

第一次握手：Client创建传输控制块`TCB`，然后向Server发出请求连接报文`SYN`，且此时SYN=1，同时选择初始序列号为seq=x。Client进入同步已发送状态(SYN-SEND)。<span style="color:red;">TCP规定，SYN=1的报文段不能携带数据，但需要消耗掉一个序号。</span>

第二次握手：Server收到Client发来的SYN,进入同步已接收状态(SYN-RECEIVED)。然后向客户端发送

# TCP四次挥手

# 服务器端：
1. 创建套接字
2. 绑定IP地址及端口号
3. 设置最大连接数
4. 等待客户端连接
5. 通信
6. 关闭连接

```csharp
Socket tcpServer = new Socket(AddressFamily.InterNetwork, 
                                SocketType.Stream, 
                                ProtocolType.Tcp);
tcpServer.Bind(
    new IPEndPoint(IPAddress.Parse("192.168.124.33"), 3366));
tcpServer.Listen(10);
Console.WriteLine("Server is Running...");

Socket client = tcpServer.Accept();
```

```csharp
//发送消息
public void SendMsg(string msg)
{
    byte[] data = Encoding.UTF8.GetBytes(msg);
    clientSocket.Send(data);
}
```

```csharp
//接收消息
public void GetMsg()
{
    while (true)
    {
        //检测连接是否断开
        if(clientSocket.Poll(10,SelectMode.SelectRead))
        {
            clientSocket.Close();
            break;
        }
        int length = clientSocket.Receive(data);
        string msg = Encoding.UTF8.GetString(data, 0, length)
        Console.WriteLine("收到消息：" + msg);
    }
}
```

# 客户端：
1. 创建套接字
2. 连接指定IP地址，端口号
3. 通信
4.关闭连接

<div align="right">TonyChenn<br> </div>