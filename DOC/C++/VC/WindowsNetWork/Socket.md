# Socket

Send等函数返回值为==SOCKET_ERROR，代表发送失败。  

Socket是单向的，服务器和客户端各自维护一个文件。  
该文件可以由本端写入，另一端读取。  
本端是在内存区域不断放入，另一端是不断从内存区域拿走。

## 初始化

```
#include <windows.h>//#include <Winsock2.h>
#pragma comment(lib,"ws2_32.lib")

WSADATA wsaData;
WORD wVersionRequested = MAKEWORD( 2, 2 );
if (0 != WSAStartup( wVersionRequested, &wsaData )) return false;
if ( LOBYTE( wsaData.wVersion ) != 2 ||HIBYTE( wsaData.wVersion ) != 2 ) WSACleanup();//清理数据
```

## TCP

+ Server

```
//创建socket
SOCKET sockSrv=socket(AF_INET,SOCK_STREAM,0);
SOCKADDR_IN addrSrv;
addrSrv.sin_addr.S_un.S_addr=htonl(INADDR_ANY);
addrSrv.sin_family=AF_INET;
addrSrv.sin_port=htons(5555); //端口号

bind(sockSrv,(SOCKADDR*)&addrSrv,sizeof(SOCKADDR));
listen(sockSrv,5); //最多能接受5个客户端链接，或SOMAXCONN

while(1)
{
    SOCKADDR_IN addrClient;
    int len=sizeof(SOCKADDR);
   //接受链接
    SOCKET sockConn=accept(sockSrv,(SOCKADDR*)&addrClient,&len);
   //发送
    char sendBuf[100]={0};
    sprintf(sendBuf,"%s",inet_ntoa(addrClient.sin_addr));
    send(sockConn,sendBuf,strlen(sendBuf)+1,0);
   //接收
    const int bufSize = 100;
    char recvBuf[bufSize ] = {0};
    recv(sockConn,recvBuf,bufSize ,0);//返回值为接收到的实际大小,网络错误时，返回SOCKET_ERROR
    printf("%s\n",recvBuf);
       //关闭
    closesocket(sockConn);
}
```

+ Client

```
客户端在第二次连接时，需要重新定义SOCKET sockClient
SOCKET sockClient=socket(AF_INET,SOCK_STREAM,0);
SOCKADDR_IN addrSrv;
addrSrv.sin_addr.S_un.S_addr=inet_addr("127.0.0.1");
addrSrv.sin_family=AF_INET;
addrSrv.sin_port=htons(5555);
if(SOCKET_ERROR==connect(sockClient,(SOCKADDR*)&addrSrv,sizeof(SOCKADDR))) return ;
//接收
const int bufSize = 100;
char recvBuf[bufSize ]={0};
recv(sockClient,recvBuf,bufSize ,0);
printf("%s\n",recvBuf);
//发送
send(sockClient,"clientSend",strlen("client")+1,0);
//关闭
closesocket(sockClient);
//WSACleanup();
```

## UDP

+ Server

```
SOCKET sockSrv=socket(AF_INET,SOCK_DGRAM,0);
SOCKADDR_IN addrSrv;
addrSrv.sin_addr.S_un.S_addr=htonl(INADDR_ANY);
addrSrv.sin_family=AF_INET;
addrSrv.sin_port=htons(5555);
bind(sockSrv,(SOCKADDR*)&addrSrv,sizeof(SOCKADDR));

SOCKADDR_IN addrClient;
int len=sizeof(SOCKADDR);
char recvBuf[100];
//接收
recvfrom(sockSrv,recvBuf,100,0,(SOCKADDR*)&addrClient,&len);
printf("%s\n",recvBuf);

closesocket(sockSrv);
WSACleanup();
```

+ Client

```
SOCKET sockClient=socket(AF_INET,SOCK_DGRAM,0);
SOCKADDR_IN addrSrv;
addrSrv.sin_addr.S_un.S_addr=inet_addr("127.0.0.1");
addrSrv.sin_family=AF_INET;
addrSrv.sin_port=htons(5555);

sendto(sockClient,"client",strlen("clinet")+1,0,(SOCKADDR*)&addrSrv,sizeof(SOCKADDR));
closesocket(sockClient);
WSACleanup();
```

## 检测socket是否可写

```
timeval tm = {0,100};
fd_set fd_write;
FD_ZERO(&fd_write);
FD_SET(*pSocket,&fd_write);
int iSelEro = select(*pSocket + 1 ,NULL,&fd_write,NULL,&tm);
```
