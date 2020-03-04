# socket

## URL

```
import java.net.InetAddress;
import java.net.UnknownHostException;

String s = "www.baidu.com";
InetAddress ts = null;
try
{
  ts = InetAddress.getByName(s);
}catch(UnknownHostException e){}
 
if(null != ts)
{
  System.out.println(s + " IP地址是 " + ts.getHostAddress());
}

//构造和解析URL
URL u = new URL("https://www.baidu.com/s?ie=utf-8&wd=baidu");

u.getProtocol() //获取协议https
u.getHost()//获取主机名www.baidu.com
u.getFile()//获取文件名s?ie=utf-8&wd=baidu

import java.io.*;
//读取当前页面的内容，用字符串读出html文件中写的内容
BufferedReader r = new BufferedReader(new InputStreamReader(u.openStream()));
String s;
while(null != (s = r.readLine()))
{
    System.out.println(s);
}
r.close();
```

## TCP

import java.io.*;  
import java.net.*;  

### 服务端

```
try {
  ServerSocket server = new ServerSocket(1234);
  while (true) {
   Socket s = server.accept();

    DataInputStream data = new DataInputStream(s.getInputStream());
    System.out.println(data.readUTF());

    data.close();


    InputStream in = socket.getInputStream();
    byte[] buf = new byte[1024];
    int len = in.read(buf);//错误时返回-1

    s.close();
    }
} catch (Exception e) {}
```

### 客户端

```
try {
  Socket s = new Socket("localhost", 1234);

  DataOutputStream data = new DataOutputStream(s.getOutputStream());
  data.writeUTF("xx");

  data.close();//socket被关闭
  s.close();
} catch (Exception e) {}
```

## UDP

import java.net.*;  
DatagramPacket用于发送和接收时，构造函数不同


### 服务端

```
try
{
   DatagramSocket dSocket = new DatagramSocket(1234);
   while(true){
    byte[]  inBuffer = new byte[100];
    DatagramPacket inPacket = new DatagramPacket(inBuffer, inBuffer.length);
   dSocket.receive(inPacket);//接收信息   
   InetAddress cAddr = inPacket.getAddress();
   int cPort = inPacket.getPort();
   String s = new String(inPacket.getData(),0,inPacket.getLength());
   System.out.println("recive: " + s);
   System.out.println("client name: " + cAddr.getHostName());
   System.out.println("client prot: " + cPort);
   }
}catch(Exception e){}

```

### 客户端

```
try {
    DatagramSocket dSocket = new DatagramSocket();
    InetAddress sAddr = InetAddress.getByName("127.0.0.1");
    String s = "xxxx";
    byte[] outBuffer = s.getBytes();
    DatagramPacket outPacket = new DatagramPacket(outBuffer,outBuffer.length,sAddr,1234);
    dSocket.send(outPacket);

      dSocket.close();
} catch (Exception e) {}
```
