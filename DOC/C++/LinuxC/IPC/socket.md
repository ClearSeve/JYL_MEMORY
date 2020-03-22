# socket

```
使用socket文件做交互媒介，后缀是.sock,类型为s

本地: PF_LOCAL 或 PF_UNIX

网络: PF_INET (IPV4)
     PF_INET6 (IPV6)

UDP    SOCK_DGRAM    打包发送  
TCP    SOCK_STREAM   数据流的方式发送
```

```
#include <stdio.h>
#include <stdlib.h>
#include <errno.h>
#include <string.h>
#include <unistd.h>
#include <netdb.h>
#include <netinet/in.h>
#include <sys/socket.h>
#include <arpa/inet.h>
```

## TCP

+ 服务器

```
int main(int argc, char *argv[])
{
    int sockSrv=socket(AF_INET,SOCK_STREAM,0);
    if (sockSrv == -1)
    {
        perror("socket创建出错！");
        return 0;
    }

    struct sockaddr_in  addrSrv;
    addrSrv.sin_family=AF_INET;
    addrSrv.sin_port=htons(5555);//端口号
    addrSrv.sin_addr.s_addr = INADDR_ANY;
    bzero(&(addrSrv.sin_zero),8);

    if (bind(sockSrv, (struct sockaddr *)&addrSrv, sizeof(struct sockaddr)) == -1)
    {
        perror("bind出错！");
        return 0;
    }

    if (listen(sockSrv, 5) == -1)//最多能接受5个客户端链接
    {
        perror("listen出错！");
        return 0;
    }


    while(1)
    {
        struct sockaddr_in addrClient;
        socklen_t sin_size = sizeof(struct sockaddr_in);

        //接受链接
        int sockConn = accept(sockSrv, (struct sockaddr *)&addrClient, &sin_size);

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
        close(sockConn);
    }

    return 0;
}
```

+ 客户端

```
int main(int argc, char *argv[])
{

    int sockClient=socket(AF_INET,SOCK_STREAM,0);

    struct hostent *host = gethostbyname("127.0.0.1");
    struct sockaddr_in addrSrv;
    addrSrv.sin_family=AF_INET;
    addrSrv.sin_port=htons(5555);
    addrSrv.sin_addr = *((struct in_addr *)host->h_addr);
    bzero(&(addrSrv.sin_zero),8);

    //连接
    if (connect(sockClient, (struct sockaddr *)&addrSrv, sizeof(struct sockaddr)) == -1)
    {
        perror("connect出错！");
        return 0;
    }

    //接收
    const int bufSize = 100;
    char recvBuf[bufSize ]={0};
    recv(sockClient,recvBuf,bufSize ,0);
    printf("%s\n",recvBuf);

    //发送
    send(sockClient,"client",strlen("client")+1,0);

    close(sockClient);

    return 0;
}
```

## UDP

### 服务器

```
int main(int argc, char *argv[])
{
    int sockSrv=socket(AF_INET, SOCK_DGRAM, 0);
    if (sockSrv == -1)
    {
        perror("socket创建出错！");
        return 0;
    }

    struct sockaddr_in  addrSrv;
    addrSrv.sin_family=AF_INET;
    addrSrv.sin_port=htons(5555);//端口号
    addrSrv.sin_addr.s_addr = INADDR_ANY;
    bzero(&(addrSrv.sin_zero),8);

    if (bind(sockSrv, (struct sockaddr *)&addrSrv, sizeof(struct sockaddr)) == -1)
    {
        perror("bind出错！");
        return 0;
    }

    while(1)
    {
        struct sockaddr_in addrClient;
        socklen_t sin_size = sizeof(struct sockaddr_in);

        //接收
        const int bufSize = 100;
        char recvBuf[bufSize ] = {0};
        int size = recvfrom(sockSrv, recvBuf, bufSize, 0, (struct sockaddr*)&addrClient, &sin_size); 
        printf("%s\n",recvBuf);

        //发送
        char sendBuf[100]= "xxxxx";
        sendto(sockSrv, sendBuf, strlen(sendBuf)+1, 0, (struct sockaddr*)&addrClient, sin_size);
    }


    close(sockSrv);

    return 0;
}
```

### 客户端

```
int main(int argc, char *argv[])
{
    int sockClient=socket(AF_INET, SOCK_DGRAM, 0);

    struct hostent *host = gethostbyname("127.0.0.1");
    struct sockaddr_in addrSrv;
    addrSrv.sin_family=AF_INET;
    addrSrv.sin_port=htons(5555);
    addrSrv.sin_addr = *((struct in_addr *)host->h_addr);
    bzero(&(addrSrv.sin_zero),8);
    socklen_t sin_size = sizeof(struct sockaddr_in);

    //发送
    sendto(sockClient ,"client",strlen("client")+1,0, (struct sockaddr*)&addrSrv, sin_size);


    //接收
    const int bufSize = 100;
    char recvBuf[bufSize ]={0};
    recvfrom(sockClient,recvBuf,bufSize ,0, (struct sockaddr*)&addrSrv, &sin_size);
    printf("%s\n",recvBuf);


    close(sockClient);

    return 0;
}
```
