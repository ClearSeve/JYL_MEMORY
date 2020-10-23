# Zlib

## 压缩

```
zlib.h
zconf.h

uLong datOrgLen = len;
uLong datDstlen = compressBound(datOrgLen);
char* buf = new char[datDstlen];
datDstlen = datDstlen;//输入输出参数
int ret = compress((Bytef *)buf, &datDstlen, (Bytef *)dat, datOrgLen);
if (ret  != Z_OK) return;
```

## 解压
```
uLong datOrgLen = len;
uLong datDstlen = len*3;//输入输出参数
char* buf = new char[datDstlen];
int ret = uncompress((Bytef *)buf, &datDstlen, (Bytef *)dat, datOrgLen);
if (ret != Z_OK) return;
```