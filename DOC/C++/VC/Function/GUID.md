# GUID

```

//guid字符串转结构体
TCHAR *strGuid = _T("{18C85E65-E783-436C-A758-94C92D90821C}");
GUID guid = {0};
CLSIDFromString(strGuid, (LPCLSID)&guid);

//创建guid
CoCreateGuid(&guid);

//guid结构体转字符串
char GuidBuf[33] = {0};
sprintf_s(GuidBuf,33,"%08X%04X%04X%02X%02X%02X%02X%02X%02X%02X%02X",
guid.Data1,guid.Data2,guid.Data3,
guid.Data4[0],guid.Data4[1],guid.Data4[2],guid.Data4[3],guid.Data4[4],guid.Data4[5],guid.Data4[6],guid.Data4[7]);

```
