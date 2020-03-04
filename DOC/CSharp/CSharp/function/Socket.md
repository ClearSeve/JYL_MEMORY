# Socket

## TCP客户端

```
private Socket sokClient = null;
private string localIP = "127.0.0.1";//服务器地址
private int localPort = 5555;//端口
private string sendStr="";//发送内容

void StartSocket()
{
  IPAddress address = IPAddress.Parse(localIP);
  IPEndPoint endpoint = new IPEndPoint(address, localPort);//把ip和端口转化为IPEndPoint实例
  sokClient = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);//创建一个Socket

  sokClient.Connect(endpoint);//连接到服务器  
  byte[] bs = Encoding.ASCII.GetBytes(sendStr);
  sokClient.Send(bs, bs.Length, 0);//发送

}
```

## TCP服务端

```
private Thread threadSocket = null;
private Socket socketServer = null;
private string ip = "127.0.0.1";
private int port = 5555;

void Start()
{
    try
    {
       socketServer = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
       IPAddress address = IPAddress.Parse(ip);
       IPEndPoint endpoint = new IPEndPoint(address, port);
       socketServer.Bind(endpoint);

       threadSocket = new Thread(SocketRev);
       threadSocket.IsBackground = true;
       threadSocket.Start();
     }
     catch (Exception ex)
     {
     }
}

void SocketRev()
{
    socketServer.Listen(1);
    Socket temp = socketServer.Accept();

    byte[] recvBytes = new byte[1024];
    int revbytes = temp.Receive(recvBytes, recvBytes.Length, 0);
    string recvStr = Encoding.ASCII.GetString(recvBytes, 0, revbytes);
 }
```

## udp服务

```
Socket socketServer = new Socket(AddressFamily.InterNetwork, SocketType.Dgram, ProtocolType.Udp);
IPAddress address = IPAddress.Parse("127.0.0.1");
IPEndPoint endpoint = new IPEndPoint(address, 5678);
socketServer.Bind(endpoint);

Thread th = new Thread(() =>
{
while (true)
      {
         EndPoint point = new IPEndPoint(IPAddress.Any, 0);//用来保存发送方的ip和端口号
         byte[] buffer = new byte[100];
         int length = socketServer.ReceiveFrom(buffer, ref point);
         string message = Encoding.Default.GetString(buffer, 0, length);
     }
});
th.IsBackground = true;
th.Start();
```
