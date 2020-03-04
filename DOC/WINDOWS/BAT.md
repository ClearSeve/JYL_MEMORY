# CMD

```
echo off
cls

:Main
echo ***********************************
echo *******     主菜单           *****
echo *******     1  fun1         *****
echo *******     2  fun2           *****
echo *******     X  exit           *****
echo ***********************************
set/p  cc=   输入命令
if /i "%cc%"=="1"   goto FUN1
if    "%cc%"=="2"   goto FUN2
if    "%cc%"=="X"   goto EX
goto Main

:EX
exit

:FUN1
echo fun11111
goto Main

:FUN2
echo fun2222
goto Main
```

## call

+ call  "***.bat"  
  执行某个bat文件


## %[1-9]

分别表示启动bat时传入的第一个到第九个参数

## @

用于一行命令，不显示命令行本身

## pause 

 暂停批处理的执行并在屏幕上显示
Press any key to continue...的提示，等待用户按任意键后继续  

## ::或rem

注释行

## if

+ if [not] "参数" == "字符串" 待执行的命令

+ if [not] exist [路径\]文件名 待执行的命令

+ if  "abc"=="abcf" ( echo "yes" )  else (echo "no")

+ if errorlevel <数字> 待执行的命令

## for

+ for %%v  in (*.bat *.txt) do type %%v   
  将当前目录下所有bat和txt文件显示出来，%%v代表执行过程中的变量

+ for   /r F:\xx    %%v in (*.txt) do (type %%v )
  遍历目录及子目录

+ for /d  %%v in (F:\*.*) do echo %%v
  只处理文件夹

+ for /L %%v in (1,2,10) do echo %%v
  1~10步长2

+ for /f  %%v  in (a.txt) do echo %%v 
  逐行读取

+ for /f "tokens=2 delims=," %%v  in (a.txt) do echo %%iv
  逐行读取,用,分割,取第二列 delims默认为空格和tab

+ for /f "tokens=2,4 delims=," %%v  in (a.txt) do echo %%v %%v 
  获取2和4列    必须使用%%i和%%j这种字母连续

+ for /f "tokens=2-4 delims=," %%i  in (a.txt) do echo %%i %%j %%k
  获取2到4列

## goto

+ goto mark  
  :mark
  跳转到mark处


## set

设置环境变量，当前窗口有效

+ set /p  val=****：
  屏幕显示 ****：后等待输入，将输入字符串赋值给val.

+ "%val%"=="a"  
   使用""将字符串包含起来进行比较
