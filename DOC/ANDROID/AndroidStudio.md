# Android Studio

## 发布

buid -》generate sigend APK  
签名类型选择V1

## 快捷键

|快捷键                            |                 说明                                   |
| :-:                             |    :-:                                                  |
|Alt + Enter                      |    解析未导入包的类或未处理异常的代码段                    |
|Ctrl + Shift + 空格              |      自动提示生成代码                                    |
|Ctrl+B                           |    跳转到函数定义（ctrl+鼠标左键）                     |
|F8                               |     下一步                                              |
|shift+F8                         |   下一个断点                                            |
|右键菜单 Column Selection Mode    |      开启竖选模式                                         |


## sdk下载
```
(HTTP Proxy选择未No proxy)

File -》Appearance&Behavior-》System Settings

http://ping.chinaz.com/dl.google.com

选择最短时间ip
x.x.x.x

C:\Windows\System32\drivers\etc\hosts
末尾添加:
x.x.x.x  dl.google.com
```

## 问题

+ 解决adb not responding if youd like to retry...错误
netstat -ano | findstr "5037"  
通过线程id，在任务管理器关闭相应的程序  

+ 编译联网问题 
在环境变量中增加_JAVA_OPTIONS 然后它的值为-Djava.net.preferIPv4Stack=true,之后打开AS就会更新gradle

+ 控件不显示
修改styles.xml  
将Theme改成Base.Theme

+解决包冲突
```
android {
     packagingOptions {
        exclude 'META-INF/DEPENDENCIES'
     }
}
```

## 文件分类

Android目录下app

+ manifests
  工程的配置，包含所有activity的定义
+ java  
  存放代码文件，通过右键创建activity可以在这里创建出类（同时会在manifests中添加定义，layout中加入xml）
+ Res  
  + drawable
    存放图片，可以使用复制，选中文件夹进行粘贴
  + Layout
    Activity文件，每个activity对应一个content_**。按钮design和Text可以切换成xml和可视化显示
  + Values
    其中定义的值，可以通过name访问。如"@string/ttt",可以访问string标签中的ttt字符串 

Java文件夹下对应activity的java代码，res/layout下xml对应界面（xml可以直接编辑，也可以使用可视化）