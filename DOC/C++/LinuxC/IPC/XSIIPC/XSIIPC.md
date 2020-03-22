# XSI IPC

```
XSI IPC包括共享内存，消息队列，信号量集，隶属于一个规范，有着共同的特征
每个XSI  IPC结构都是在内核中存储和维护的，外部到内核要用key，内核中使用id标识
key_t  key有三种方式得到：
1）使用宏 IPC_PRIVATE做key，无法实现进程间通信（私有），极少使用
2）把所有的key定义在一个头文件中，用宏定义
3）使用ftok()函数生成key，参数：真实存在的路径和项目编号（0到255）
 id的生成：ipc结构在内核中都用id做唯一标识，创建、获取id都有对应的函数
key_t  key=ftok("/home/jyl",222);//生成一个key


调用**get()时，都有一个flags，创建时值为：
权限 | IPC_CREAT或IPC_EXCL
 IPC_CREAT - 不存在则创建，存在则获取
 IPC_EXCL - 如果存在则创建失败


IPC结构都有一个特殊的操作函数，提供查询，修改和删除功能
函数名：**ctl()  如：shmct(),msgctl()
参数cmd提供功能：
IPC_STAT   查属性、状态
IPC_SET    修改权限
IPC_RMID   删除ipc结构，按id删除
```
