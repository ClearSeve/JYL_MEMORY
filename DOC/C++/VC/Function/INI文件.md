# INI文件

```
通过设置段名和键名可以确定一个键值。可以进行写入和读取。
Ini文件写入情况如下（也可以用section的函数写没有键名的）
不能新建txt然后改成ini，第一次文件需要用函数写。文件名必须是全路径，不能是相对路径。
[Section1]
1=x
2=y
[Section2]
abc

::WriteProfileString等SDK函数可以在C:\WINDOWS\win.ini读写数据。如果是WriteProfileString等CWinApp的成员函数，则会写入注册表中。
```

## 写入

```
::WritePrivateProfileString(strSectionName, strKeyName, strKeyValue, strFileName);
如果strKeyValue是NULL，则表示删除那一行

没有strKeyName时(debug下会有冗余数据，只有release可以)：
::WritePrivateProfileSection(strSectionName,strKeyValue,strFileName);
```

## 读取

```
CString strKeyValue = _T("");
::GetPrivateProfileString(strSectionName, strKeyName, _T(""), 
	strKeyValue.GetBuffer(1024), 1024,strFileName);
strKeyValue.ReleaseBuffer();

没有strKeyName时：
::GetPrivateProfileSection(strSectionName,strKeyValue.GetBuffer(1024),1024,strFileName);

读取数字：（只用于带key的）
int x=GetPrivateProfileInt(strSectionName,strKeyValue,NULL,strFileName);
```

## 遍历

```
void GetUnitINI(map<CString,CString> &Key_Value,CString strSection,CString iniFileName)
{ 
	while(true)
	{
		CString strKey;
		DWORD  nChar = GetPrivateProfileString(strSection,NULL,_T(""),strKey.GetBuffer(1024),1024,iniFileName);
		if (0 == nChar) break;


		CString strValue;
		GetPrivateProfileString(strSection,strKey,_T(""),strValue.GetBuffer(1024),1024,iniFileName);
		strValue.ReleaseBuffer();

		Key_Value[strKey] = strValue;

		//删除
		WritePrivateProfileString(strSection,strKey,NULL,iniFileName);
	}

	//重新写入
	for (map<CString,CString>::const_iterator iter = Key_Value.begin(); iter != Key_Value.end(); ++iter)
	{
		WritePrivateProfileString(strSection,iter->first,iter->second,iniFileName);
	}
}
```
