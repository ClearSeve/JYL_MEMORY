# Activity

## 创建
鼠标右键工程文件夹可以new一个activity，自动生成对应的java文件和xml

1. Res/layout文件夹右键new-》layout resource file创建myaty.xml文件，对应页面
2. java文件夹中创建Aty类extends Activity，重写onCreate函数，setContentView(R.layout.myaty);
3. Manifests文件夹中xml进行注册：<activity android:name=".Aty"></activity>
   其中.Aty表示和manifest中package属性中文件夹位置拼凑出的Aty对应java文件的路径

setContentView是每个Activity类构造函数中需要调用的，用来绑定界面xml。传入的参数也可以是控件（控件也是Activity）。

## 启动

### 类名启动

```
假设在MainActivity类中启动一个aty
Intent in = new Intent(MainActivity.this,aty.class);
startActivity(in); //启动Activity
```

### 字符串启动

```
<activity android:name=".Myaty" android:exported="true">
    <intent-filter>
        <category android:name="android.intent.category.DEFAULT"></category>
        <action android:name=" AtyName "></action>
    </intent-filter>


可以通过字符串跨程序启动
android:exported="true"   默认为ture，表示可以被其他程序启动
<action android:name="AtyName"></action>    AtyName表示Activity标志字符串


=============启动
try {
    startActivity(new Intent("AtyName "));
}catch (Exception e){}



=============名称相同
如果两个Activity字符串名称相同，则打开时会进行选择询问

<action android:name="AtyName"></action>  后面加入：
<data android:scheme="sss"></data>

startActivity( new Intent("AtyName ",Uri.parse("sss://")));
启动时就会自动筛选      //后面是参数

```

### 网页启动Activity
```
<activity android:name=".Myaty" android:label="aaa">
    <intent-filter>
        <category android:name="android.intent.category.BROWSABLE"/>
        <category android:name="android.intent.category.DEFAULT" />

        <action android:name= "android.intent.action.VIEW" />
        <data android:scheme="app"></data>
    </intent-filter>
</activity>

网页标签
<a href="app://">open</a>

类中可以获取网页传递的启动参数
Uri uri = getIntent().getData();
```

## 打开网页
startActivity(new Intent(Intent.ACTION_VIEW,Uri.parse("https://www.baidu.com/")));

## 传递参数

### 传递单个参数
```
启动activity之前加入
in.putExtra("dat","hello");

activity中oncreate中获取
Intent i = getIntent();
String  info = i.getStringExtra("dat");
```


### 传递数据包

```
启动activity之前加入
in.putExtra("name","aa");
in.putExtra("id",1);

或者
Bundle b = new Bundle();
b.putString("name","aa");
b.putInt("id",1);

in.putExtras(b);
//可以是in.putExtra("dat",b);  通过Bundle  b =   i.getBundleExtra("dat");获取


activity中oncreate中获取
Intent i = getIntent();
Bundle  b =   i.getExtras();

String name = b.getString("name");
int   id = b.getInt("id");
int   age = b.getInt("age",20);//默认值20
```

### 传递自定义数据1
```
使用java的序列化，操作简单，速度慢
public class UserDat implements Serializable {
    String name = "aa";
    UserDat(String name)
    {
        this.name = name;
    }
}


加入数据
in.putExtra("user",new UserDat("xxx"));


获取数据
UserDat  dat =  (UserDat) i.getSerializableExtra("user");

```

### 传递自定义数据2

```
使用andorid接口，操作复杂，速度快

public class UserDat implements Parcelable {
    String name = "aa";
    UserDat(String name) {
        this.name = name;
    }


//可以通过Alt + Enter自动实现（直接选择类名，Add Parcelable implement可以之直接全部自动实现）
    @Override
    public int describeContents() {
        return 0;
    }
    @Override
    public void writeToParcel(Parcel dest, int flags) {
        dest.writeString(name);
    }



    protected UserDat(Parcel in) {
        name = in.readString();
    }
    public static final Creator<UserDat> CREATOR = new Creator<UserDat>() {
        @Override
        public UserDat createFromParcel(Parcel in) {
            return new UserDat(in);
        }

        @Override
        public UserDat[] newArray(int size) {
            return new UserDat[size];
        }
    };


}


传递数据
in.putExtra("user",new UserDat("aaa"));

接收数据
UserDat  dat =  (UserDat) i.getParcelableExtra("user");

```

## 接收activity返回值

### 启动activity

```
Intent in = new Intent(MainActivity.this,aty.class);
startActivityForResult(in,0);//0代表请求码requestCode


实现虚函数用于接收返回值
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);

    String info = data.getStringExtra("dat");
}
```

### Activity中退出并设置返回值

```
Intent in = new Intent();
in.putExtra("dat","xxxxx");
setResult(1,in);//1为返回码resultCode
finish();//关闭当前activity
```

## 属性

获取当前Activity的id和名称
int taskId = getTaskId();
String curActivity = toString();

## 启动方式
Manifest.xml中可以配置Activity启动方式，默认是standard
<activity android:name=".MainActivity" android:launchMode="standard">

### Standard

堆栈方式，新建的Activity向堆栈中放入（创建新实例），当点击后退时，退出当前Activity，显示之前的一个Activity。
taskId相同，不同堆栈位置不同。

### singleTop
如果当前Activity在栈顶，如当前Activity启动自己，则不会创建新的实例

### singleTask
一个任务堆栈只包含Activity一个实例，可以包含多个Activity

### singleInstance
一个任务堆栈只包含一个Activity一个实例

### 运行流程
```
onCreate()方法是启动Activity地默认调用的方法

setContentView(R.layout.activity_main);
设置启动的activity

运行流程包括的函数：
onCreate    onStart   onResume   onPause   onStop     onDestroy   onRestart
启动时：onCreate    onStart   onResume 
点击最小化或进入后台时：onPause   onStop
再次进入：onRestart   onStart   onResume
退出activity：onPause   onStop  onDestroy   

在一个activity1中启动另一个activity2时，先执行activity1的onPause ，再执行activity2的onCreate    onStart   onResume，当activity2完全呈现时，再执行activity1的onStop
activity2退出时，先执行onPause，当activity1执行完onRestart   onStart   onResume之后，activity2再执行onStop  onDestroy  
```
