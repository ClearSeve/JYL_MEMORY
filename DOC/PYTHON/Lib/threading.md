# threading

线程中不能修改全局变量  
import threading

## 开启线程
传入类的__thrRun函数作为线程入口  
_thread = threading.Thread(target=self.__thrRun)  
_thread.setDaemon(True)  
_thread.start()  

setDaemon默认为False，如果使用默认值，则进程结束后，线程依然执行  
如果设置为True，则进程结束时线程停止  

## 线程等待
等待_thr1线程执行完成，如果传递参数，则只等待传递的时间  
self._thr1.join()  

## 暂停
import time

单位为s
time.sleep(1)

## 线程同步

可重入锁，防止死锁
```
from threading import RLock

lock = RLock()
lock.acquire()
lock.acquire()
...执行代码
lock.release()
lock.release()
```