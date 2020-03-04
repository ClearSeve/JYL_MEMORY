# COM组件

右键解决方案增加 用户控件（非wpf那个） UserControl1  
工具栏选择项增加WMP控件，将控件拖入UserControl1的窗口

UserControl1的cs文件中加入接口函数  
public void play(String mediaPathName)  
{  
     this.axWindowsMediaPlayer1.URL = mediaPathName;  
}  



调用：  
xaml中拖入 WindowsFormsHost控件  
标签加入Name属性为host1  


UserControl1 f = new UserControl1();  
host1.Child = f;  
f.play("D:\\xxx.wmv");
