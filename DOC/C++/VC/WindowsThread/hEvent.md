# hEvent

```
HANDLE hEvent;  hEvent=CreateEvent(NULL,TRUE,FALSE,NULL);//TRUE代表创建的是手动事件，FALSE代表创建时未占用（ResetEvent状态,内核占用该对象）

SetEvent将对象从内核拿出并占用
ResetEvent将对象还给内核
多次调用SetEvent(hEvent)不会有影响
```

## 事件控制

```
DWORD WINAPI fun(LPVOID p)
{
	while (true)
	{
		WaitForSingleObject(hEvent, INFINITE);
	
		//要运行的代码

		ResetEvent(hEvent);
	}
	return 0;
}


判断处于ResetEvent状态（说明已经执行到线程末尾的ResetEvent处，线程处于空闲状态），然后再控制线程开始运行：
if (WAIT_TIMEOUT == WaitForSingleObject(hEvent, 0))
{  SetEvent(hEvent);   }

WAIT_OBJECT_0   ：核心对象已被激活
WAIT_TIMEOUT   ：等待超时
WAIT_FAILED  ：出现错误
````

## 信号控制

```
可以被多个线程调用
相当于bool值,通过HasSignal()判断有无信号

struct Signal
{
	Signal()
	{
		m_hEvent = CreateEvent(NULL, TRUE, FALSE, NULL);
	}
	bool HasSignal()
	{
		return WAIT_TIMEOUT != WaitForSingleObject(m_hEvent,0);
	}
	void SetSignal()
	{
		SetEvent(m_hEvent);
	}
	void DelSignal()
	{
		ResetEvent(m_hEvent);
	}
	HANDLE m_hEvent;
};
```
