# MySql

## centeros安装mysql

```

//相关依赖安装
yum install -y perl perl-Module-Build net-tools autoconf libaio numactl-libs 
//下载安装包
wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm

//检查安装情况
yum repolist enabled | grep "mysql.*-community.*"

//安装
yum install mysql-community-server

//启动服务
systemctl start mysqld
//重启服务
systemctl restart mysqld

//查看启动状态
systemctl status mysqld

//开机启动
systemctl enable mysqld
systemctl daemon-reload

//修改密码策略
在/etc/my.cnf文件中可以关闭密码策略，修改后重启mysql
validate_password = off

//查看默认密码
grep 'temporary password' /var/log/mysqld.log
//登录
mysql -uroot -p
//修改密码
set password for 'root'@'localhost'=password('#Abcdefg!');


//修改默认编码
show variables like '%character%';
查看字符编码

/etc/my.cnf文件，[mysqld]下添加配置
[mysqld]
character_set_server=utf8
init_connect='SET NAMES utf8'


//usr使用密码psd远程登录
GRANT ALL PRIVILEGES ON *.* TO 'usr'@'%' IDENTIFIED BY 'psd' WITH GRANT OPTION;
```

## windows下卸载

HKEY_LOCAL_MACHINE\\SYSTEM\\ControlSet001\\Services\\Eventlog\\Application\\MySQL文件夹删除

C:\\ProgramData\\MySQL删除

删除C:\\Documents and Settings\\All Users\\Application Data\\MySQL

## 导入db

source d:\\rmt.sql

## 登录

mysql -u root -p密码

mysql -h30.158.59.78 -u用户名 -p密码



## 创建数据库

```
create database dbname;
use dbname;
```

## 修改密码

set password = password('密码');
flush privileges;

## 用户管理

以root用户登录

### 创建用户

insert into mysql.user(Host,User,Password)
values("localhost","abc",password("1234"));

insert into mysql.user(Host,User,Password)  values("192.168.125.133","abc",password("1234"));//为192.168.125.133创建用户abc

flush privileges;

### 删除用户

DELETE FROM mysql.user WHERE User="abc" and Host="localhost";

### 授权

```

//授权user用户拥有dbname数据库的所有权限。
grant all privileges on dbname.* to user@localhost identified by '1234';
flush privileges;

//指定部分权限给一用户
grant select,update on dbname.* to user@localhost identified by '1234';
flush privileges;

### 修改用户密码
update mysql.user set password=password("new password") where User="abc" and
Host="localhost";
flush privileges;
```

## 数据类型

int,integer,bigint  
float,double  
char,varchar  
datetime: 2020-1-1 17:00:00  
date:2020-1-1
time:17:00:00
