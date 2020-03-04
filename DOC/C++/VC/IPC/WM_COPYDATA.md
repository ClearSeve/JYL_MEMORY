# WM_COPYDATA

发送WM_COPYDATA后，要等客户端执行该消息函数结束后  
::SendMessage(hWnd,(UINT)WM_COPYDATA,0,(LPARAM)&cds);代码之后的才执行  
WM_COPYDATA传输只读数据

## 发送端

```
HWND  hWnd=::FindWindow(NULL,_T("name")); //接收端窗口名
COPYDATASTRUCT cds;
cds.dwData=1;//用户定义数据
cds.lpData="abcd";//数据指针
cds.cbData=5;//数据长度
::SendMessage(hWnd,(UINT)WM_COPYDATA,(WPARAM)m_hWnd,(LPARAM)&cds);
```

## 接收端

处理WM_COPYDATA消息
```
BOOL C**Dlg::OnCopyData(CWnd* pWnd, COPYDATASTRUCT* pCopyDataStruct) 
{
	CStringA  str = (LPSTR)pCopyDataStruct->lpData;

	return CDialog::OnCopyData(pWnd, pCopyDataStruct);
}

//PCOPYDATASTRUCT lpcds=(PCOPYDATASTRUCT)lParam;
//(LPSTR)lpcds->lpData
```