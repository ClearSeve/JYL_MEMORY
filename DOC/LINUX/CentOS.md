# CentOS

查看系统版本  
cat /etc/redhat-release


## yum

查看已安装的软件
yum list installed

查看已安装的java软件
yum list installed|grep java

卸载软件  
yum remove ABC


## 开机启动

开机自启  
systemctl enable ABC  

关闭开机自启  
systemctl disable ABC  
