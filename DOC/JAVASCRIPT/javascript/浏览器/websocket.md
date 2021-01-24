# websocket
```
<script>
    var addr = "ws://127.0.0.1:8081/con1"; //ip:port可以使用window.location.host
    var ws = new WebSocket(addr);
    ws.onopen = function () {
    };
    ws.onclose = function (event) {
    };
    ws.onerror = function() {  
    } 
    ws.onmessage = function (event) {
    	console.log(event.data);
    };

    //ws.send("msg");
</script>
```