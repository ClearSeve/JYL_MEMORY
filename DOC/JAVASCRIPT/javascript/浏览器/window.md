# window

```
window对象是一个浏览器对象，全局，表示浏览器目前正打开的窗口，是其他对象的顶层对象，所以可以省略名称，直接调用

location.host           当前页面的主机ip端口地址，如：127.0.0.1:8080

location.href           window.location.protocol + "//" + window.location.host + "/page.html";  当前窗口进行页面跳转

alert                   弹出提示框  alert(....);  

confirm                 弹出确认框,确认返回true  confirm(...);

prompt                  弹出提示对话框，并要求输入一个字符串  prompt("xx","默认值");

open                    var windowVar = open("b.html","target","width=500,height=200");  
                        执行成功返回新窗口对象。第一个参数为url地址，空字符串则打开空窗口。第二个参数制定新窗口名称，该名称可作为<a>和<form>的target属性。第三个参数可选。

close                   关闭浏览器窗口  close();

setInterval             定时器，1s调用1次fun  setInterval("fun",1000);  
clearInterval           清除定时器，id从1开始，是每个定时器设置的先后次序  clearInterval(1);

setTimeout              setTimeout("fun()",1000); 1s后执行fun函数
clearTimeout            clearTimeout(1); 取消代码延迟

print                   调用浏览器打印机 print();

moveTo                  移动浏览器到指定位置  moveTo(1,1);

moveBy                  移动偏移量  moveBy(-10,100);

scrollTo                移动右侧和下方滚动条

resizeTo                设置窗口宽高  resizeTo(500, 500);

innerHeight             显示区的高度

innerWidth              显示区的宽度
```
