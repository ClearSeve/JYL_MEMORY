# CString

```
#include <atlstr.h>
CString以”\0”结尾，如果字符串中在结尾前包含”\0”,那么CString以第一次出现”\0”为准。
LPCTSTR 指的就是CString，LPSTR指char *   
可用CString  str(_T("xxx"));构造
可用str.LoadString(ID)加载字符串资源中的字符串
str.Format(_T("define %d %d"),wParam,lParam);
str.Empty(); 清空字符串
str+_T(’c’);   为字符串增加一个字符
```

## 追加

str.Append(_T("x"));  
str.AppendFormat(_T("%d"),10);

## 查找
if(-1!=str.Find(_T("clear")))//表示str中有clear字样,如果find返回-1代表没有}

## 分割

```
CString str = _T("ab,cde");  
int pos = str.Find(_T(","));

str = str.Left(pos);//得到ab
	
str = str.Mid(pos+1);//得到cde,第二个参数如果不设置，代表从pos位置截取到末尾（加1因为查找的字符串为个长度）
或者：str = str.Right(str.GetLength()-pos-1); 
```

## 修剪

CString str=_T("   ab");  
str.TrimLeft(); //可以将字符串左边多余的空格去掉

## 按条件截取

```
CString str=_T("123abcd456");
CString s1=str.SpanIncluding(_T("0123456789"));//s1为123
CString s2=str.SpanIncluding("a");//s2为空
CString s3=str.SpanExcluding(_T("d"));//s3为123abc
```

## 比较

str1.Compare(str2);   //如果str1与str2一样，返回0，否则返回1  
str1.CompareNoCase(str2);  //比较时忽略大小写

## 转换
char *p=strA.GetBuffer(0);//CStringA中的GetBuffer就是转成char *;GetBuffer后必须要调用ReleaseBuffer，否则CStringA的其他函数失效。  
int  i=_ttoi(p); //字符串变成int

转成wchar_t
int i=::MultiByteToWideChar(CP_ACP,0,(LPCTSTR)file,-1,NULL,0);
wchar_t *pPath=new wchar_t[i];
::MultiByteToWideChar(CP_ACP,0,(LPCTSTR)file,-1,pPath,i);
delete[] pPath;

```
//GBK -> UTF-8 
std::string GBKToUTF8(const std::string& strGBK)
{
	std::string strOutUTF8 = "";
	WCHAR * str1;
	int n = MultiByteToWideChar(CP_ACP, 0, strGBK.c_str(), -1, NULL, 0);
	str1 = new WCHAR[n];
	MultiByteToWideChar(CP_ACP, 0, strGBK.c_str(), -1, str1, n);
	n = WideCharToMultiByte(CP_UTF8, 0, str1, -1, NULL, 0, NULL, NULL);
	char * str2 = new char[n];
	WideCharToMultiByte(CP_UTF8, 0, str1, -1, str2, n, NULL, NULL);
	strOutUTF8 = str2;
	delete[]str1;
	str1 = NULL;
	delete[]str2;
	str2 = NULL;
	return strOutUTF8;
}
```

## CString转换
CStringW strw = _T("abc");
CStringA str(strw.GetBuffer());
string  s(str.GetBuffer());
