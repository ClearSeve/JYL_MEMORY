# Vmware

## 安装
安装时，需要关闭vs  

安装VMware Tools（VM菜单中），可以调节屏幕大小,设置分辨率即可改变大小  

Ctrl+Alt        ： 退出鼠标  
Ctrl+Alt+回车   ： 切换全屏  

虚拟机ip地址一定要使用自动获取，主机上虚拟机的网络适配器ip地址也自动获取。  
虚拟机只能用客户端程序，向主机发命令，不能做服务端。  

主机可能需要先设置密码，在虚拟机中登录主机（需要输入密码）  

## 主机共享路径

虚拟机关闭状态下  
VM虚拟机开启界面  
Edit virtual machine settings  
Options ->Shared folders  
选择always enabled  
点击ADD，选择主机需要共享的文件夹路径  

打开虚拟机，安装VMware Tools（VM菜单中）
在计算机磁盘显示中，有可移动存储设备中，会出现vmware tools，双击进行安装

右键菜单添加网络位置
浏览文件夹，在网络中添加wmware-host

## 虚拟机不能联网等问题

打开windows服务，开启vm相关服务
