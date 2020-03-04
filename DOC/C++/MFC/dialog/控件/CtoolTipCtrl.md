# CtoolTipCtrl

```
可以为指定控件（窗口）增加提示，当鼠标移动到控件上时，弹出提示。
类内变量:
CToolTipCtrl m_tt;
 
初始化:
EnableToolTips(TRUE); 
m_tt.Create(this); 
m_tt.Activate(TRUE); 
m_tt.AddTool(GetDlgItem(ID),"提示内容"); 

PreTranslateMessage:
m_tt.RelayEvent(pMsg); 
```
