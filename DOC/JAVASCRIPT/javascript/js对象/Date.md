# Date

```
date = new Date();  获取当前时间
new Date(2010,1,1);  构造，小时，分钟，秒，毫秒缺省

date.getSeconds() 获取秒数
date.getFullYear()获取年，getYear()获取的是19**年差

显示时间
 <p id = "clock"></p>
function show()
{
    var now = new Date();
    clock.innerHTML = now.getHours() + ":"  + now.getMinutes() + ":" + now.getSeconds();
}
window.setInterval("show()",1000);
```
