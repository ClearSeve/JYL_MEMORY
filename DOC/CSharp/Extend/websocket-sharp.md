# websocket-sharp
```
using WebSocketSharp;
using WebSocketSharp.Server;


//client
using (var ws = new WebSocket("ws://127.0.0.1:8081/con1"))
{
    ws.OnMessage += (sender, e) =>
    {
        Console.WriteLine(e.Data);
    }
    
    ws.Connect();

    ws.Send(dat);
}


//server
public class OnMsgRev : WebSocketBehavior
{
    protected override void OnMessage(MessageEventArgs e)
    {
        if(e.IsText)
        {
            Console.WriteLine(e.Data);
            Send(e.Data);
        }
               
        if(e.IsBinary)
        {
            var dat = e.RawData;
        }
    }
}

var wssv = new WebSocketServer("ws://127.0.0.1:8081");
wssv.AddWebSocketService<OnMsgRev>("/con1");//OnMsgRev监听con1的连接、断开、数据消息
wssv.Start();
Console.ReadKey(true);
wssv.Stop();
```