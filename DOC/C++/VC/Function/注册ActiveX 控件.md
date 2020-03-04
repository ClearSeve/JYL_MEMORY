# 注册ActiveX 控件

```
BOOL DllRegister(LPCTSTR lpszDllName)
{
	ASSERT(lpszDllName != NULL);
	ASSERT(AfxIsValidString(lpszDllName));

	//加载ActiveX控件
	HINSTANCE hLib = LoadLibrary(lpszDllName);
	if (hLib == NULL)
	{
		TRACE(_T("%s加载失败\n"), lpszDllName);
		return FALSE;
	}

	//获得注册函数DllRegisterServer地址
	FARPROC lpDllEntryPoint; 
	lpDllEntryPoint = GetProcAddress(hLib, _T("DllRegisterServer"));

	//调用注册函数DllRegisterServer
	if (lpDllEntryPoint != NULL)
	{
		if (FAILED((*lpDllEntryPoint)()))
		{
			TRACE(_T("调用DllRegisterServer失败\n"));
            FreeLibrary(hLib);
            return FALSE;
		}
		else
		{
			FreeLibrary(hLib);
			return TRUE;
		}
	}
	else
	{
		TRACE(_T("调用DllRegisterServer失败\n"));
		FreeLibrary(hLib);
		return FALSE;
	}
}
```
