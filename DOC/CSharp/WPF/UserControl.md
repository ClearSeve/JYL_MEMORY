# UserControl

## 用户控件创建

新建 UserControl1 
命名空间 UserSpace


代码中包含：
public int userVal{ get; set; }  
public void Fun()  
{  
    int y = userVal + 10;  
}  

## 调用


```
<Window .........  
    xmlns:NNN ="clr-namespace:UserSpace"   
     ..........>  

<NNN:UserControl1  x:Name="userCtl"  userVal="123"/>


调用用户控件中的方法:  
userCtl.Fun();
```

```
引用其他dll中的控件时，需要clr-namespace:***;assembly=**
xmlns:Controls="clr-namespace:MahApps.Metro.Controls;assembly=MahApps.Metro"  
``` 
## 容器

```
<ContentPresenter  Content="{Binding CONTENT_PAGE}"/>

UserControl _content;
public UserControl CONTENT_PAGE
{
    get { return _content; }
    set
    {
        _content = value;
        RaisePropertyChanged("CONTENT_PAGE");
    }
}
```

CONTENT_PAGE = userCtl;


## 动态加载xaml
```
string strXaml = "<Button xmlns='http://schemas.microsoft.com/winfx/2006/xaml/presentation'" + 
                " Width='128' Height='32'>" + 
                " STR </Button>"; 
StringReader strreader = new  StringReader(strXaml); 
XmlTextReader xmlreader = new  XmlTextReader(strreader);
object obj = XamlReader.Load(xmlreader);
CP.Content = (UIElement)obj;
```
