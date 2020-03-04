# 与c++交互

## 对象注入

```
类中声明属性调用函数
Q_PROPERTY(QString name READ getName WRITE setName)

//代码中将对象加入上下文
#include <QQmlContext>
QQmlApplicationEngine engine;
MyData data;
QQmlContext *ptx = engine.rootContext();
ptx->setContextProperty("msg",&data);

//qml调用
msg.name = "abc";//调用setName 函数
var val = msg.name;//调用getName 函数
```

## 注册qml对象

```
class MyData:public QObject
{

    Q_OBJECT
    Q_PROPERTY(QString name  READ getName WRITE setName NOTIFY nameChanged)
public:
    void setName(const QString &name)
    {
        if(m_name != name)
        {
            m_name = name;
            emit nameChanged(name);
        }
    }
    QString getName()const{
        return m_name;
    }
signals:
    void nameChanged(const QString &name);
    //函数首字母必须小写，对应槽函数onNameChanged

private:
    QString m_name;



    //注册枚举类
    Q_ENUMS(Status)
public:
    enum Status
    {
        Status_A,
        Status_B,
    };
    Q_INVOKABLE Status getStatus() //注册可以被qml调用的函数，函数首字母必须小写
    {
        return Status_A;
    }
};



//代码中注册
qmlRegisterType<MyData>("jyl.data", 1, 0, "MyData");

//qml调用
import jyl.data  1.0
MyData{
    id:myd
    onNameChanged: function(name)
    {//可以不写function(name)
        console.debug(name);
    }
}

myd.name = "abc";//调用setName 函数
var val = myd.name;//调用getName 函数

//调用getStatus()函数
var ss = myd.getStatus();
if(ss === 0) console.log("Status_A");
```
