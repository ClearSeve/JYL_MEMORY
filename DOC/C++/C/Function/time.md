# time

```
time.h
time_t  t=time(0);  可获得当前时间(从1970年1月1日0:0:0开始的秒差)
显示时间：
while(1){
time_t   t=time(0);
printf("…\r＂,t);
fflush(stdout);同一位置显示时间，不断刷新
while(t==time(0));等待1秒
}
s=t%60;
m=t%3600/60;
h=(t%(3600*24)/3600+8)%24
ctime(&t) 转成美式时间  %s

char buf[100]={0};
struct tm* cur=localtime(&t);
sprintf(buf,”%4d”,cur->tm_year+1900);

tm_year+1900
tm_mon+1
tm_mday
tm_hour
tm_min
tm_sec

sleep() 
Exp: int a=sleep(3)
使程序休眠3秒，中断时返回剩余为休眠的秒数
```
