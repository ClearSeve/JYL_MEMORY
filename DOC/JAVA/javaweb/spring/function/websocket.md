# websocket

## POM
```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-websocket</artifactId>
</dependency>
```

## ws.html
```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"/>
</head>
<body>
   <textarea id="txtContent" cols="50" rows="10" readonly="readonly"></textarea>
</body>
<script th:inline="javascript">
    /*<![CDATA[*/
    var cid = /*[[${cid}]]*/;
    /*]]>*/
</script>
<script>
    var ws = new WebSocket("ws://"+window.location.host+"/ws/" + cid);
    ws.onopen = function () {
    };
    ws.onclose = function (event) {
    };
    ws.onerror = function() {  
    } 
    ws.onmessage = function (event) {
    	document.getElementById("txtContent").value = event.data;
    	console.log(event.data);
    };
</script>
</html>
```

## WebSocketConfig.java

```

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.socket.server.standard.ServerEndpointExporter;

@Configuration 
public class WebSocketConfig {
	@Bean
    public ServerEndpointExporter serverEndpointExporter() {  
        return new ServerEndpointExporter();  
    }  
}

```

## WebSocketServer.java

```
import java.io.IOException;
import java.util.concurrent.CopyOnWriteArraySet;
import java.util.concurrent.atomic.AtomicInteger;

import javax.websocket.OnClose;
import javax.websocket.OnError;
import javax.websocket.OnMessage;
import javax.websocket.OnOpen;
import javax.websocket.Session;
import javax.websocket.server.PathParam;
import javax.websocket.server.ServerEndpoint;

import org.springframework.stereotype.Component;


import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@ServerEndpoint("/ws/{clientId}")
@Component
public class WebSocketServer {
	
	private static final Logger LOGGER = LoggerFactory.getLogger(WebSocketServer.class);
	
	public static AtomicInteger onlineCount = new AtomicInteger(0);
    
    //线程安全Set，用来存放每个客户端对应的MyWebSocket对象。
    private static CopyOnWriteArraySet<WebSocketServer> webSocketSet = new CopyOnWriteArraySet<WebSocketServer>();

    private Session session;

    private String clientId="";


    @OnOpen
    public void onOpen(Session session,@PathParam("clientId") String id) {
        this.session = session;
        webSocketSet.add(this);    
        addOnlineCount();         
        LOGGER.info("有新窗口开始监听:"+id+",当前在线人数为" + getOnlineCount());
        this.clientId=id;
        try {
        	 sendMessage("连接成功");
        } catch (IOException e) {
        	LOGGER.error("websocket IO异常");
        }
    }

    @OnClose
    public void onClose() {
        webSocketSet.remove(this);  
        subOnlineCount();           
        LOGGER.info("有一连接关闭！当前在线人数为" + getOnlineCount());
    }
 
    @OnMessage
    public void onMessage(String message, Session session) {
    	LOGGER.info("收到来自窗口"+clientId+"的信息:"+message);
        for (WebSocketServer item : webSocketSet) {
            try {
                item.sendMessage(message);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }


    @OnError
    public void onError(Session session, Throwable error) {
    	//LOGGER.error("发生错误");
        //error.printStackTrace();
    }
    
    
	
    void sendMessage(String message) throws IOException {
        this.session.getBasicRemote().sendText(message);
    }



    
    public static void sendMsg(String message,String cid) throws IOException {
    	LOGGER.info("推送消息到窗口"+cid+"，推送内容:"+message);
        for (WebSocketServer item : webSocketSet) {
            try {
            	//只推送给cid，为null则全部推送
            	if(cid==null) {
            		item.sendMessage(message);
            	}else if(item.clientId.equals(cid)){
            		item.sendMessage(message);
            	}
            } catch (IOException e) {
                continue;
            }
        }
    }

    public static int getOnlineCount() {
        return onlineCount.get();
    }
    public static void addOnlineCount() {
        WebSocketServer.onlineCount.incrementAndGet();
    }
    public static void subOnlineCount() {
        WebSocketServer.onlineCount.decrementAndGet();
    }
}
```

## WSController.java

```
import java.io.IOException;

import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class WSController {		
    @RequestMapping("/login/{clientId}")
    public String  login(ModelMap map,@PathVariable("clientId") String clientId) {
    	map.addAttribute("cid", clientId);
    	return "ws";//ws.html
    }
    
    
    @RequestMapping("/msg/{clientId}/{msg}")
    public @ResponseBody String sendMessage(@PathVariable("clientId") String clientId,@PathVariable("msg") String msg) {   	   	
    	try {
    		WebSocketServer.sendMsg(msg, clientId);
		} catch (IOException e) {
			e.printStackTrace();
		}
        return "";
    }

}
```

## 使用
```
浏览器登录：http://127.0.0.1:8080/login/jyl
(jyl为cid)

ws.html定义连接地址："ws://"+window.location.host+"/ws/" + cid
对应@ServerEndpoint("/ws/{clientId}")

另一浏览器发送http://127.0.0.1:8080/msg/jyl/abc

```