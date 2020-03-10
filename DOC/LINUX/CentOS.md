# CentOS

查看系统版本  
cat /etc/redhat-release


## yum

查看已安装的软件
yum list installed

查看已安装的java软件
yum list installed|grep java

## python

+ 安装

```
yum install -y zlib zlib-devel

tar -zxvf  ...tgz
进入解压目录
./configure --prefix=路径/python
make
make install

环境变量：路径/python/bin
```

+ 删除

```
rpm -qa|grep python|xargs rpm -ev --allmatches --nodeps
whereis python |xargs rm -frv
whereis python
```
