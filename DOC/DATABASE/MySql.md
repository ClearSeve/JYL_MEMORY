MySql
=====

配置
----

### 安装

环境变量：

path加入C:\\Program Files\\mysql-8.0.13-winx64\\bin

将mysql-8.0.13-winx64目录放入C:\\Program Files

将my.ini放入mysql-8.0.13-winx64目录

=================my.ini===========

[mysql]

default-character-set=utf8

[client]

port=3306

default-character-set=utf8

[mysqld]

port=3306

basedir="C:\\Program Files\\mysql-8.0.13-winx64"

datadir="C:\\Program Files\\mysql-8.0.13-winx64\\data"

max_connections=200

character-set-server=utf8

default-storage-engine=INNODB

default_authentication_plugin=mysql_native_password

运行intall.bat：

cd C:\\Program Files\\mysql-8.0.13-winx64\\bin

mysqld --initialize-insecure

mysqld --install MySQL --defaults-file="C:\\Program
Files\\mysql-8.0.13-winx64\\my.ini"

net start mysql

pause

登录：

mysql -u root -p

use mysql;

update user set authentication_string='' where user='root';

ALTER user 'root'\@'localhost' IDENTIFIED BY '新密码'

【非mysql_native_password时：

ALTER USER 'root'\@'localhost' IDENTIFIED WITH mysql_native_password BY '新密码'

FLUSH PRIVILEGES;

】

卸载

net stop mysql

sc delete MySQL

### 卸载

HKEY_LOCAL_MACHINE\\SYSTEM\\ControlSet001\\Services\\Eventlog\\Application\\MySQL文件夹删除

C:\\ProgramData\\MySQL删除

删除C:\\Documents and Settings\\All Users\\Application Data\\MySQL

导入
----

进入db

source d:\\rmt.sql

### 服务器运行

=======启动服务器

管理员模式运行cmd

net start mysql

=======停止

net stop MySQL

### 登录

mysql -u root -p（第一次登录没有密码，直接按回车过）

mysql -h30.158.59.78 -u用户名 -p\*\* //远程登录 \*\*代表密码

### 查看状态

mysql\> status

### 修改密码

mysql\> set password =password('密码');

mysql\> flush privileges;

用户管理
--------

以root用户登录

### 创建用户

insert into mysql.user(Host,User,Password)
values("localhost","abc",password("1234"));

insert into mysql.user(Host,User,Password)
values("192.168.125.133","abc",password("1234"));

//为192.168.125.133创建用户abc

flush privileges;

### 删除用户

DELETE FROM mysql.user WHERE User="abc" and Host="localhost";

### 授权

//为用户创建一个数据库

create database db;

//授权phplamp用户拥有phplamp数据库的所有权限。

grant all privileges on phplampDB.\* to phplamp\@localhost identified by '1234';

//刷新系统权限表

flush privileges;

//指定部分权限给一用户

mysql\>grant select,update on db.\* to abc\@localhost identified by '1234';

//刷新系统权限表

mysql\>flush privileges;

### 修改用户密码

update mysql.user set password=password("new password") where User="abc" and
Host="localhost";

mysql\>flush privileges;

创建/使用数据库
---------------

create database \*\*\*;

use \*\*\*;

数据类型
--------

int,integer,bigint

float,double

char,varchar

datetime: 2017-12-12 17:00:00

date:2017-12-12

time:17:00:00

密码
----

md5(“password”) 根据字符串创建密码
