# Servece

## 特性
程序在退出Activity界面时在系统运行任务中可以看到，如果停止服务，则退出界面时系统运行任务中看不到。
工程文件夹右键new –》Serve自动创建Service类，以及在manifests中加入service标签。与Activity相同。
onCreate和onDestroy会在server被创建和被停止时调用


由startService启动则只能用stopService停止
由bindService启动则只能用unbindService停止
如果Service已经创建，则调用函数时只执行onStartCommand（MyServer中）或者onServiceConnected（Activity中）

## 启动停止
启动：startService(new Intent(MainActivity.this,MyService.class));
停止：stopService(new Intent(MainActivity.this,MyService.class));

onStartCommand 函数在startService每次调用时执行

## 绑定
bindService(new Intent(MainActivity.this, MyService.class), MainActivity.this, Context.BIND_AUTO_CREATE);

实现ServiceConnection接口，onServiceConnected在第一次bindService时调用
onServiceConnected(ComponentName name, IBinder service)


MyService类中：
public IBinder onBind(Intent intent) {
    return  new Binder();
}


解除绑定：unbindService(MainActivity.this);

如果server不存在或已经解绑，则unbindService会抛出异常

====================================  
可以通过IBinder接口实现外部直接操作Myserver（在Myserver类中创建一个类MyBinder，MyBinder直接通过Myserver.this访问。外部通过将IBinder转换为Myserver进行访问）

=========MyService 类中

public IBinder onBind(Intent intent){ return new MyBind();}
public class MyBind extends  Binder
{
    public void Fun(){ MyService.this.Fun();}
}
void Fun(){System.out.println("fun");}


=========Activity中
public void onServiceConnected(ComponentName name, IBinder service) {
    MyService.MyBind bind = (MyService.MyBind) service;
    bind.Fun();
}


## 跨应用的Servece

### 启动参数

通过包名和类名地址，可以跨应用启动服务  

```
com.***是包名（manifest 中package属性
MyService是类名
Intent intent = new Intent();
intent.setComponent(new ComponentName("com.*** ","com.***.MyService")); 
```

### 启动

startService(intent);

### 绑定
生成或加入AIDL文件后先进行重新编译

New   AIDL文件  自动生成IMyAidlInterface文件
MyServer中

```
public IBinder onBind(Intent intent) {
    return new IMyAidlInterface.Stub() {
        @Override
        public void basicTypes(int anInt, long aLong, boolean aBoolean, float aFloat, double aDouble, String aString) throws RemoteException {

        }
    };
}
```

绑定操作
bindService(intent,MainActivity.this, Context.BIND_AUTO_CREATE);


================= 通信
将其他程序的AIDL文件原样拷贝到当前工程  
1. New-》Folder-》AIDL Folder
2. 为AIDL Folder中创建包，包名和其他程序的AIDL文件包名一样
3. 将AIDL文件拷贝

IMyAidlInterface bind = IMyAidlInterface.Stub.asInterface(iBinder);