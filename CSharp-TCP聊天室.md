---
title: CSharp-TCP聊天室
date: 2019-04-13 13:22:25
tags:
    - TCP
    - CSharp
description: Winform实现聊天室，包含群发，一对一聊天，一对多聊天，包含客户端，服务器端两部分。
img: https://raw.githubusercontent.com/TonyChenn/BlogPicture/master/2019/04.13/icon.jpg
---
# 前言:
TCP是一种面向连接的，可靠的，基于字节流的传输层通讯协议。记录下Winform基于TCP协议的连天室。

# 介绍
## 服务器端
### 事件类型包括上线，下线，广播消息，一对一消息，
```csharp
public enum EventType
{
    Exit = 0,       //下线
    Online,         //上线
    BroadCast,      //群发
    OneToOne,       //一对一，一对多
}
```

### 封装客户端类，包含客户端套接字，客户端名称，发送消息，接收消息。
```csharp
class Client
{
    //套接字
    private Socket client;
    //名称
    public string name;
    private byte[] data = new byte[1024];

    public bool IsConnected
    {
        get { return client.Connected; }
    }

    public Client(Socket client)
    {
        this.client = client;
        //开启线程接收消息
        Thread thread = new Thread(RecvMsg);
        thread.Start();
    }
    public void SendMsg(string msg)
    {
        client.Send(Encoding.UTF8.GetBytes(msg));
    }
//接收到消息后将消息转发给所有客户端
    public void RecvMsg()
    {
        while (true)
        {
            try
            {
                int length = client.Receive(data);
                string msg = Encoding.UTF8.GetString(data, 0, length);
                Console.WriteLine("收到：" + msg);
                string[] _data = msg.Split(':');
                this.name = _data[1];
                Program.BroadCastMsg(msg);
            }
            catch(Exception ex)
            {
                Console.WriteLine(ex.Message);
                client.Shutdown(SocketShutdown.Both);
                client.Close();
                break;
            }
        }
    }
}
```
### 创建tcp连接，开启线程监听客户端的连接，当有客户端连接后，将连接的客户端存入一个列表中。每次向客户端发送消息时检测是否有断开的连接，如果有就从列表中移除
```csharp
static void Main(string[] args)
{
    server = new Socket(
        AddressFamily.InterNetwork, 
        SocketType.Stream, 
        ProtocolType.Tcp);
    server.Bind(
        new IPEndPoint(IPAddress.Parse("127.0.0.1"), 3399));
    server.Listen(10);
    Console.WriteLine("服务器正在运行...");
    Thread th = new Thread(WatchingClient);
    th.Start();
}

//监听客户端的连接
private static void WatchingClient()
{
    while (true)
    {
        Socket cc = server.Accept();
        Client client = new Client(cc);
        Console.WriteLine("新的客户端连接成功");
        clientList.Add(client);
    }
}
```
### 当客户端上线后会给服务器端发送一个上线口令，当服务器检测到该口令后通知所有客户端更新在线列表，并把在线用户列表发给所有用户

客户端发来的口令格式为：
|类型|描述|格式|
|---|---|---|
|Online|上线|"Online:"+用户名|
|Exit|下线|"Exit:"+用户名|
|BroadCast|群发消息|"BroadCast:"+消息内容|
|OneToOne|私聊消息|"OneToOne:"+"用户1^用户2^...^用户n:"+消息内容|

```csharp
//转发消息
public static void BroadCastMsg(string msg)
{
    RemoveLogOutUser();
    string[] data = msg.Split(':');
    switch(data[0])
    {
        case "Online":
            foreach (Client item in clientList)
            {
                if (item.IsConnected)
                    item.SendMsg(EventType.Online + ":" + SerializeUser());
            }
            break;
        case "OneToOne":
        case "BroadCast":
        case "Exit":
            foreach (Client client in clientList)
            {
                if (client.IsConnected)
                    client.SendMsg(msg);
            }
            break;
    }
}
```
## 客户端
### 界面设计
待补充
### 登录
输入用户名后，进行登录，连接服务器，告诉服务器自己上线。
```csharp
//登录操作
private bool Login()
{
    IPAddress ip = IPAddress.Parse("127.0.0.1");
    client = new Socket(
            AddressFamily.InterNetwork,
            SocketType.Stream,
            ProtocolType.Tcp);
    try
    {
        client.Connect(new IPEndPoint(ip, 3399));
        client.Send(Encoding.UTF8.GetBytes(EventType.Online + ":" + uid));
        MessageBox.Show("登陆成功");
        return true;
    }
    catch(Exception ex)
    {
        MessageBox.Show("登录失败！！"+ex.Message);
        return false;
    }
}
```
### 下线，告诉服务器自己要下线了，然后关闭连接
```csharp
private void Btn_logout_Click(object sender, EventArgs e)
{
    try
    {
        client.Send(Encoding.UTF8.GetBytes(EventType.Exit + ":" + uid));
        client.Close();
    }
    catch(Exception ex)
    {
        new Exception("注销失败"+ex.Message);
    }
    finally
    {
        Environment.Exit(0);
    }
}
```
### 对接收到服务器端发来消息的处理及响应
由于Winform中默认无法在线程中操作UI,所以需要特殊的方法，当然方法不止一种。
```csharp
private void RecvMsgHandler(string msg)
{
    string[] data = msg.Split(':');
    string method = data[0];
    Thread thread = null;
    switch(method)
    {
        case "Online":
            //开启线程，更新在线用户列表
            thread = new Thread(new ParameterizedThreadStart(AddOnLineUser));
            thread.Start(data[1]);
            break;
        case "OneToOne":
            if (DeSerializeUser(data[1]).Contains(uid))
            {
                thread = new Thread(new ParameterizedThreadStart(UpdateChatMsg));
                thread.Start(data[2]);
            }
            break;
        case "BroadCast":
            thread = new Thread(new ParameterizedThreadStart(UpdateChatMsg));
            thread.Start(data[1]);
            break;
        case "Exit":
            thread = new Thread(new ParameterizedThreadStart(RemoveOnLineUser));
            thread.Start(data[1]);
            break;
    }
}

// 只例举线程中更新UI中的更新在线用户列表
private void AddOnLineUser(object obj)
{
    if(Lb_online_user.InvokeRequired)
    {
        Dele d = new Dele((obj1) =>
        {
            Lb_online_user.Items.Clear();

            List<string> cur = DeSerializeUser(obj1.ToString());
            foreach (string item in cur)
            {
                Lb_online_user.Items.Add(item);
            }
        });
        Lb_online_user.Invoke(d, obj);
    }
}
```

### 发送消息
根据不同的信息类型发送不同信息口令

```csharp
private void Btn_Send_Click(object sender, EventArgs e)
{ 
    if(SendMessageType.BroadCast==sendType && NullOrEmpty(Tb_edit.Text))
        MessageBox.Show("发送的消息不能为空");
    if((SendMessageType.OneToOne==sendType && GetUserCheckedCount()==0) || NullOrEmpty(Tb_edit.Text))
        MessageBox.Show("发送的消息不能为空,发送成员不能为空");
    else
    {
        switch(sendType)
        {
            case SendMessageType.BroadCast:
                client.Send(
                    Encoding.UTF8.GetBytes(
                        EventType.BroadCast + ":" + uid + "群发：" + Tb_edit.Text));
                break;
            // 消息类型 : 接收者 ：消息
            case SendMessageType.OneToOne:
                client.Send(
                    Encoding.UTF8.GetBytes(
                        EventType.OneToOne + ":" + 
                        SerializeUser()+ ":" + 
                        uid + "私发：" + Tb_edit.Text));
                break;
        }
        Tb_edit.Text = "";
    }
}
```

# 完整代码
链接:https://pan.baidu.com/s/1nDbiIxLaBPveQKLg00UMYw  密码:h6kd