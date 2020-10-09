# UserControl

## 用户控件创建

新建<font color="#0000FF">UserControl1</font>  
命名空间<font color="#FFFF00">UserSpace</font> 


代码中包含：
public int <font color="#FF0000">userVal </font>{ get; set; }  
public void <font color="#00FFFF">Fun</font>()  
{  
    int y = userVal + 10;  
}  

## 调用

<Window .........  
    xmlns:<font color="#00FF00">NNN</font> ="clr-namespace:<font color="#FFFF00">UserSpace</font>"   
     ..........>  

<<font color="#00FF00">NNN</font>:<font color="#0000FF">UserControl1</font>  x:Name="<font color="#FF00FF">userCtl</font>"  <font color="#FF0000">userVal</font>="123"/>


调用用户控件中的方法:  
<font color="#FF00FF">userCtl</font>.<font color="#00FFFF">Fun()</font>;

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

CONTENT_PAGE = <font color="#FF00FF">userCtl</font>;


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
