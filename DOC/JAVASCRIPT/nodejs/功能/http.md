# 服务搭建

```
var http = require('http');
var url = require("url");
http.createServer(function (request, response) {
	var pathname = url.parse(request.url).pathname;
        response.writeHead(200, {'Content-Type': 'text/plain;charset=utf-8'});
        response.end('*********');
}).listen(8080);
console.log('Server running at http://127.0.0.1:8080/');
```

将内容写入ser.js文件  
命令行启动服务： node ser.js  

## 参数获取

```
var parseObj = url.parse(request.url, true);

request.method       //请求方式 GET/POST
parseObj.pathname   //无参数的url路径
request.headers["content-type"]    //消息报头中的key-val
parseObj.query['x']    //参数x的值


var qs = require('querystring');
//获取post参数
if("POST" == request.method.toUpperCase())
{
var postData = "";
request.addListener("data", function (data) {
postData += data;
});
request.addListener("end", function () {
var query = qs.parse(postData);
console.log(query['passWord']);
});
}
```
