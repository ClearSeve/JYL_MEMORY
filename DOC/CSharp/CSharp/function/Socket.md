# Socket

## TCP客户端

```
IPAddress address = IPAddress.Parse("127.0.0.1");
int port = 1234;
IPEndPoint endpoint = new IPEndPoint(address, port);
Socket sokClient = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);

sokClient.Connect(endpoint);
byte[] bs = Encoding.UTF8.GetBytes("aaaaa");
sokClient.Send(bs, bs.Length, 0);
```

## TCP服务端

```
void Start()
{
    try
    {
       Socket socketServer = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
       IPAddress address = IPAddress.Parse("127.0.0.1");
       int port = 1234;
       IPEndPoint endpoint = new IPEndPoint(address, port);
       socketServer.Bind(endpoint);

       Thread threadSocket = new Thread(SocketRev);
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
