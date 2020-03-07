# ActiveX

## 导出

```
**.idl文件末尾：
[
	uuid(注册的uuid)
]
coclass ocx名称
{
	[default] dispinterface _D**名称;
	[default, source] dispinterface _D**Events;
};

需要注册32位控件，才能在设计窗口使用
```

## 显示处理

OnDraw函数中可以将原来绘图的代码注释掉，写入自定义的显示代码。

调用Invalidate();或者InvalidateControl();可以让窗口重新刷新显示。

## 消息处理

可以为C**Ctrl类加入消息响应，如WM_CREATE中初始化，WM_TIMER处理定时器消息（注意在写SetTimer(1,1000,NULL);这种函数时候，第一个参数其实已经包含，可能会出现代码提示帮助错误）

## 与外界交互

### 属性页

```
在C**Ctrl的cpp中有关于属性页的绑定函数：
BEGIN_PROPPAGEIDS(C***Ctrl, 1)
	PROPPAGEID(C***PropPage::guid)
END_PROPPAGEIDS(C***Ctrl)
如果要增加属性页中的页面，可以在这里加入绑定，加入后数量从原来的1，变为2。
如增加颜色属性页：
PROPPAGEID(CLSID_CColorPropPage)

ClassWizard内Automation页面中，选择Add Property，这个页面中的External name可以输入一个名称定义一个属性，也可以选择增加一些常规的属性。
```

### 常规属性

```
Implementation选择Stock表示选择的属性是控件的标准常规属性。添加后可以在控件的属性页面中看到这个属性并且可以设置。程序中可以通过函数获取这个设置的属性，然后对控件进行响应属性改变。

增加常规属性BackColor，后可以在属性设置页面中设置这个值，在程序中通过：
COLORREF ref=TranslateColor(GetBackColor());
CBrush brush(ref);
pdc->FillRect(rcBounds,&brush);
可以获取颜色，然后使用设置的颜色设置控件背景色。
pdc->SetBkMode(TRANSPARENT)可以使上面的文字背景色透明。;
```

### 自定义变量/属性

```
Implementation选择Member variable，表示增加一个变量，同时会生成相关函数（如果选择Get/Set Methods只能生成函数，还需要自己加入成员变量来保存该属性变量）

External name是提供给外部程序调用时使用的变量名。如x;
Type可以自定义选择变量类型，如short。
Variable name是控件内部程序对于这个变量使用的名字。如m_x;
Notification function中对应的是函数名，该函数在外部程序改变这个变量时自动调用。在该变量设置后，外部使用这个控件会看到关于该属性的设置和获取的函数。
```

### 属性值保存

```
   在对话框中通过属性页设置了属性后，可以保存下来，下次打开工程时，查看控件的属性时可以是修改后的值。
在CXXXCtrl的DoPropExchange函数中：
PX_Short(pPX,"x",m_x,100);  //初始值为100
```

### 属性值通知

```
自定义了属性值，在改变后，需要让容器也知道属性值改变了，以便让容器的属性面板中的相应值进行改变。
在属性值改变的响应函数中加入代码：
BoundPropertyChanged(0x01);
数字代表在_D**中自动生成的变量id号码。如 [id(1)] short  x;
```

### 运行状态

使用if(AmbientUserMode()){...}可以判断控件是在运行时还是在容器中处于设计状态。可以使有些代码的运行只在程序运行后执行。

### 属性页对话框

可以在属性页的对话框资源中加入编辑框等控件，然后绑定变量（和C***PropPage绑定的），这时有一个Option Property name，可以关联到常规属性或者自定义属性。这样在属性页中可以修改自定义或者常规的属性。

## 与外界连通的函数

ClassWizard内Automation页面中选择Add Message。可以设置函数的名称，返回值以及参数列表（参数的类型有限制）。确认后会在_D***中生成相应外部函数接口代码。在C**Ctrl类中，则是函数的声明和实现。

## 事件

可以为控件增加某些消息事件，会在_D***Events中生成相应的代码。增加后，控件就可以响应该消息，并将该消息发送给容器。比如鼠标点击控件，则对话框可以收到控件被点击的消息。

可以增加标准事件，也可以增加自定义事件，自定义事件后，会生成一个函数（而且会为控件生成一个消息名称，在外部可以响应这个消息）。在控件中调用该函数时候，就可以向外部发出消息
