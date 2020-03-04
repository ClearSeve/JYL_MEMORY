# Tomcat

## web路径

访问页面地址：http://127.0.0.1:8080/jyl/  
页面中包含连接：resources/jyl.jpg，等价于http://127.0.0.1:8080/jyl/resources/jyl.jpg

## 配置

```
webapps中存放web项目文件  

conf下server.xml中 <Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443"/>

定义连接端口,默认8080
```

## 运行

```
运行环境需要配置环境变量：JRE_HOME

可以通过tomcat下bin目录中startup.bat启动
默认打开index文件

http://127.0.0.1:8080/webPrjName/
项目的根目录就是该地址（项目的webapp目录）

导出WAR文件，放在tomcat的webapps目录下，使用bin目录下startup.bat运行

tomcat运行环境的conf目录下server.xml  Host标签可以设置项目文件目录:
<Context docBase="E:\apache-tomcat-9.0.0.M21\webapps\webTest" path="/webTest" reloadable="true" source="org.eclipse.jst.jee.server:webTest"/>
```

## Eclipse配置

```
==============加入tomcat环境
Window->Preferences->server->runtime environment 

add...  apache Tomcat v9.0
设置tomcat installation directory(bin目录上级)

===============web项目配置
右键工程build path     libraries页签   

add libray...
Server Runtime      选择tomcat版本
JRE System Library  选择jre版本
```

## IDEA配置

```
不需要安装tomcat，会自动自动下载tomcat插件相关相关内容

pom.xml:
<plugin>
    <groupId>org.apache.tomcat.maven</groupId>
    <artifactId>tomcat7-maven-plugin</artifactId>
    <version>2.2</version>
    <configuration>
        <port>8080</port>
        <path>/</path>
        <uriEncoding>UTF-8</uriEncoding>
        <server>tomcat7</server>
     </configuration>
</plugin>


Edit Configurations..  -> + ->Maven
Name: web
Command line:tomcat7:run
```

## WEB-INF

```
resources目录下内容会拷贝到WEB-INF\classes

WEB-INF目录下的文件无法直接通过URL访问

通过在web.xml中配置，可以映射目录下的文件到根目录下，使其可以访问

spring-servlet.xml中：
<mvc:resources mapping="/resources/**" location="/WEB-INF/classes/"/>
/resources/icon/a.png映射为/WEB-INF/classes/icon/a.png
```

