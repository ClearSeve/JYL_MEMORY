# TABcontrol

## 设置

```
控件需要的页面的对话框如：IDD_DIALOG1，IDD_DIALOG2
设置属性：Border none ，Style Child
建立对话框类
dlg1，dlg2

主对话框使用时注意包含页面对话框的头文件
绑定变量
CTabCtrl m_Table;
类内变量
dlg1 page1;
dlg2 page2;
```

## 初始化

### 设置页面文字

m_Table.InsertItem(0,_T("页面1"));  
m_Table.InsertItem(1,_T("页面2"));

### 设置控件属性（非必须）

```
m_Table.MoveWindow(30,30,200,200);
m_Table.SetMinTabWidth(30); // 设置标签项的最小宽度 
m_Table.SetPadding(CSize(30,8)); // 设置标签项和图标周围的间隔 
```

## 创建页面

page1.Create(IDD_DIALOG1,&m_Table);  
page2.Create(IDD_DIALOG2,&m_Table);

### 页面显示位置

```
将其覆盖在Tab控件的相应位置上
只能在创建页面后使用
CRect rect; 
m_Table.GetClientRect(&rect); 
rect.top+=20; 
rect.bottom-=5; 
rect.left+=5;
rect.right-=5; 
page1.MoveWindow(&rect);
page2.MoveWindow(&rect);
```

### 设置选择

```
page1.ShowWindow(SW_SHOW); //初始化选择

m_Table.SetCurSel(1);
```

## 选择消息处理

```
为Tab控件的TCN_SELCHANGING消息添加ON_NOTIFY的相应消息处理
在函数中为不同页签的选择添加相应显示
int CurSel=m_Table.GetCurSel(); 
switch(CurSel) 
{
case 0: 
	page1.ShowWindow(SW_SHOW); 
	page2.ShowWindow(SW_HIDE); 
	break; 
case 1:
	page1.ShowWindow(SW_HIDE); 
	page2.ShowWindow(SW_SHOW); 
	break; 
default: ; 
} 
```
