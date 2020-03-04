# Application

## Activity包含

application标签中，如果某个Activity包含：
```
<intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
</intent-filter>
```
则这个Activity类就是app启动时加载的

如果一个程序有多个Activity，且每个都包含以上标签，则会创建多个启动图标，每个图标启动对应的Activity。
<activity android:name=".aty2" android:label="aty2">
Label可以设置启动图标的说明文字

## App类使用
通过app类共享数据
```
public class App extends Application {
    public String strDat;
}
manifests中application标签增加属性android:name=".App"
```

此时程序启动加载不使用默认的Application类， 使用App类。
可以通过((App)getApplicationContext()).strDat 在Activity之间共享数据

App类中的onCreate()函数在每次Activity启动时都会先进行调用