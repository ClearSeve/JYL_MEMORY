# CPropertySheet

## 数据交互

可以在sheet中声明一些变量，在page中对这些变量进行赋值，在sheet的DoModal之后用sheet的对象取出这些数据。

## 设置

```
class CMyPropertySheet : public CPropertySheet
//属性表单，用来联系属性页

建立两个对话框(可以选择IDD_PROPPAGE_LARGE这种类型的对话框资源[注意资源语言的选择]，而不使用默认的)
为两个对话框建立对话框类，注意继承自CPropertyPage
class CPage1 : public CPropertyPage
class CPage2 : public CPropertyPage
```

## 初始化

```
注意包含头文件
CMyPropertySheet sheet(_T("xx"),this);
CPage1 p1;
CPage2 p2;
sheet.AddPage(&p1);
sheet.AddPage(&p2); //按顺序加入，代表向导中顺序
sheet.SetWizardMode();//设置为向导模式
   //如果不设置向导模式，可以和TABcontrol有相同显示效果
```

## 显示

```
sheet.SetActivePage(&p2); //设置要激活显示的页面
sheet.DoModal();
///如果是Wizard模式，DoModal的返回值为ID_WIZFINISH 或者IDCANCEL.
```

## 修改属性

### 修改整体属性

```
需要为CMyPropertySheet添加OnInitDialog等函数
//隐藏掉帮助按钮
GetDlgItem (IDHELP)->ShowWindow (FALSE);
//隐藏掉取消按钮
GetDlgItem(IDCANCEL)->ShowWindow(FALSE);
```

### 修改单页面属性

```
每个页面包含的按钮以及按钮的可用性不同，需要在CPage1这种页面的类中（注意包含头文件）实现虚函数OnSetActive。
第一个页面中：
((CMyPropertySheet*)GetParent())->SetWizardButtons(PSWIZB_NEXT);
最后一个页面中：
((CMyPropertySheet*)GetParent())->SetWizardButtons(PSWIZB_FINISH|PSWIZB_BACK);
中间步骤页面中：
((CMyPropertySheet*)GetParent())->SetWizardButtons(PSWIZB_BACK|PSWIZB_NEXT);
```

### 阻止进入下一页

```
为需要的page页增加虚函数OnWizardNext
if(***){ ***  return -1;}
```
