# socket

## 服务器

```
const net = require( 'net' );

const server = new net.createServer();
server.on('connection', (client) => {

  client.on('data', function (msg) { 
    console.log(msg.toString());
  });

  client.on('error', function (e) {
    console.log('client error' + e);
    client.end();
  });
  client.on( 'close', function () {
    console.log('disconnect');
  });

  //client.destroy()
});

server.listen(1234,'127.0.0.1',function () {
  console.log('start run listen');
});
```

## 客户端

```
const net = require( 'net' );

const socket = new net.Socket();
socket.connect( 1234,'127.0.0.1',function(){
    socket.write('client send msg');// string| Buffer| Uint8Array
  });

socket.on( 'data', function ( msg ) {
  console.log( msg );
});

socket.on( 'error', function ( error ) {
  console.log( 'error' + error );
});
socket.on('close',function(){
  console.log('close');
});

//socket.destroy()
```
