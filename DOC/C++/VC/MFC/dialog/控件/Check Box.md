# Check Box

## 使用

绑定变量或使用IsDlgButtonChecked(m_hWnd, IDC_CHECK1)进行勾选判断

```
//GetCheck判断勾选状态，SetCheck设置勾选
if(1==((CButton*)GetDlgItem(IDC_CHECK1))->GetCheck()){
		((CButton*)GetDlgItem(IDC_CHECK1))->SetCheck(0);
}
else{
		((CButton*)GetDlgItem(IDC_CHECK1))->SetCheck(1);
}
```

## 代码创建CheckBox

```
CButton *pBtn = new CButton();
RECT rect = {x,y,x+width,y+height};
pBtn->Create(_T("标题"),WS_TABSTOP|WS_VISIBLE|WS_CHILD|BS_AUTOCHECKBOX,rect,this,id);
```
