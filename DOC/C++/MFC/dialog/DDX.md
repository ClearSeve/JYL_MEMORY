# DDX

不可用于线程中  

将对话框的控件与对话框类的成员变量绑定，可以通过访问成员变量方式去操作对话框控件  
virtual  void  DoDataExchange(CDataExchange  *pDX)    
   包含一系列的绑定函数（使用参数pDX），绑定过程可自动生成    

绑定函数：以DDX开头  
DDX_Text(......)  值类型的绑定  
DDX_Control(.....)  控件类型的绑定  

UpdateData(BOOL)  在控件与成员变量进行数据交换时调用  
不能在线程中用这个函数，线程中只能用SendMessage方式让主线程刷新。  

TRUE  将用户在控件中输入的值传递给变量，得到数据（默认，一般可不写）  
FALSE  将变量的值显示到控件上（编辑框用）  

控件绑定的变量不能写在对话框类的初始化函数或者WM_CREATE里面，只能写在InitDialog函数或者之后的执行函数里面。  

## 控件类型的绑定

1. 类内定义控件类  
CButton  m_btnOK;  //或者写成CWnd  m_btnOK;  

2. 在DoDataExchange函数中绑定  
DDX_Control(pDX,IDOK,m_btnOK);
 DoDataExchange中传入  与ok按钮绑定   链接到类内成员

3. 使用绑定的控件进行操作（比如在InitDialog函数中）

```
m_btnOK.SetWindowText(“xxx”);
m_btnOK.MoveWindow(0,0,100,100);通过变量修改控件
return  TRUE;
```

## 值类型绑定

1. 类内定义变量CString  str;   也可以绑定int类型  
2. DoDataExchange函数中  
   DDX_Text(pDX,IDC_EDIT1,str);  与编辑框绑定  
   

UpdateData(TRUE);   
可将编辑框输入的内容取出到str中。TRUE为默认值，可以缺省

str="xxxx";  
UpdateData(FALSE);  显示到控件 
