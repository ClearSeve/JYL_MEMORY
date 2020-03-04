# CComboBox

支持用户的输入和选择，可下拉  
（加入时将高度拉高，修改时需要点击下拉按钮，然后把鼠标放到控件下方中间点才可修改）

在属性中有数据属性，用;写表示换行  
如果属性里样式类型改为下拉列表（drop  list），就只能选择，不能接受输入
默认为数据自动排序

m_combox.EnableWindow(BOOL) 设置组合框是否可用  
int nSel=m_combox.GetCurSel();//得到当前选择的位置，如果没有选择则为-1,发生选择事件时，界面文字未变化，只有选择编号变化
m_combox.GetLBText(nSel,str);//获取选择位置的数据 

m_combox.DeleteString(0);//删除当前剩余数据的第一个