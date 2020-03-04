# SOCKET

## TCP

+ 服务器

```
from socket import *
address='127.0.0.1'     #127.0.0.1是监听本机 0.0.0.0是监听整个网络
port=12345

buffsize=1024
s = socket(AF_INET, SOCK_STREAM)
s.bind((address,port))
s.listen(1)     #最大连接数1
while True:
    clientsock,clientaddress=s.accept()
    print('connect from:',clientaddress)
    while True:  
        recvdata=clientsock.recv(buffsize).decode('utf-8')
        if  not recvdata:
            break
        senddata=recvdata+'from sever'
        clientsock.send(senddata.encode())
    clientsock.close()
s.close()
```

+ 客户端

```
from socket import *
address='127.0.0.1'  
port=12345

buffsize=1024
s=socket(AF_INET, SOCK_STREAM)
s.connect((address,port))
while True:
senddata="xxxx"
s.send(senddata.encode())

recvdata=s.recv(buffsize).decode('utf-8')
print(recvdata)

s.close()
```

## UDP

+ 服务器  

```
from socket import *
HOST = '192.168.1.3'
PORT = 9999

s = socket(AF_INET, SOCK_DGRAM)
s.bind((HOST, PORT))
while True:
    data, address = s.recvfrom(1024)
    print(data, address)
s.sendto('this is the UDP server',address)  
s.close()
```

+ 客户端
```
from socket import * 
HOST='192.168.1.3' 
PORT=9999 

s = socket(AF_INET,SOCK_DGRAM) 
s.connect((HOST,PORT)) 
while True: 
message = raw_input('send message:>>') 
s.sendall(message) 
data = s.recv(1024) 
s.close() 
```