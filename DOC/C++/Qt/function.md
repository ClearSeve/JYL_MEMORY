# function

## 下载地址

http://download.qt.io/archive/qt/

## 消息响应

```
A类中有SIGNAL函数sendSigFun()
B类中有SLOT函数RevSigFun()
将A对象的信号函数与B对象的槽函数绑定
QObject::connect(&Aobj,SIGNAL(sendSigFun()),
     &Bobj,SLOT(RevSigFun()));


消息函数的声明宏必须放在头文件中
函数绑定时一定不能写形参名，只能写类型
class AA :public QObject {
    Q_OBJECT   //信号槽类需要声明宏
signals:       //信号函数以signals:指示字符开始
    void sendSigFun(const char*);


public:
    void Proc(const char* str) {
        emit sendSigFun(str);//信号函数调用前加emit
    }
};


class BB :public QObject
{
    Q_OBJECT            //信号槽类需要声明宏
    private slots:      //private slots：指示字符开始
    void  RevSigFun(const char* str) {
        qDebug()<<str;
    }
};



QObject::connect(&Aobj,SIGNAL(sendSigFun(const char*)),
                &Bobj,SLOT(RevSigFun(const char*)));
```

## 线程

```
class WorkerThread : public QThread
{
    void run() override {
    }
};


WorkerThread m_WorkerThread;
m_WorkerThread.start();
```

## QDebug

qDebug()<<*****

## QString

+ 构造函数

```
QString()
QString(const char* str)  
QString(const QChar *unicode, int size = -1)
```

+ 类型转换

```
int  a = str.toInt();
QString str=QString::number(12)
```

+ 格式化字符串  
str = QString::asprintf("%d,%.3f",12,12.123);

+ 字符串查找

```
int indexof(QString str);
int indexof(char  ch);  	未找到返回-1
```

## QList

```
QList<int>  list;
MyChar  ch=list.first();
foreach(int,   list){}遍历，每次将list中的成员放入ch

list.count()
```

## QTimer

```
QTimer  *timer=new QTimer(&obj);
timer->setInterval(1000);
QObject::connect(timer,SIGNAL(timeout()),&obj,SLOT(RevSigFun()));
timer->start();
```
