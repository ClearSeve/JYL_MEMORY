# JSP元素

```
========指令标识，java代码，jsp表达式使用
<%@ page import = "java.text.SimpleDateFormat" %>
<%@ page import = "java.util.Date" %>
<%
Date date = new  Date();
SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
String  time = simpleDateFormat.format(date);
%>

body中可以通过jsp表达式直接使用java代码：
curTime=<%=time %>

=======jsp代码嵌套html
body中可以用java代码逻辑嵌套html
<%  if(false){  %>
<p>true</p>
<%  }else{      %>
<p>false</p>
<%  }           %>
```

## java代码

```
<%   ****  %>
当前页面有效，页面关闭时销毁
```

## 指令标识

```
 <%@  指令名  属性1="val1" %>  
 ```

## page

```
定义整个jsp页面相关属性，指令属性包括：
language：  <%@ page language="java" %>   目前只包含java
extends：所有jsp页面在执行之前都会被服务器解析成servlet，而servlet是由java类定义的，所以jsp和servlet都可以继承指定的基类。该属性可以设置jsp页面继承的java类。
import：用于导入java代码使用的java类包 <%@page import="java.util.List" %>
pageEncoding：指定页面编码格式
contentType：设置jsp页面的MIME类型和字符编码  <%@ page contentType="text/html; charset=utf-8"%>

session：指定当前页面是否使用session会话对象，默认true
buffer：设置jsp的out输出对象使用的缓冲区大小，默认8kb，单位只能使用kb。
autoFlush：设置jsp页面缓存满时是否自动刷新。默认值true。如果使用false，则缓冲满时抛出异常。
isErroPage：可以将当前jsp页面设置成错误处理页面来处理另一个jsp页面的错误。
errorPage：指定一个目标jsp页面来处理当前页面错误，目标jsp页面必须设置isErroPage属性为true。该属性的值是一个url。
```

## include

```
可以引入另一个jsp,将另一个jsp在当前页面展开合并为一个jsp.
<%@ include file=”**.jsp” %>
被包含的jsp除了必要内容，只包含<%@ page  pageEncoding="utf-8"%>即可
```

## taglib

可以引入标签库

## JSP表达式

```
<%=  表达式 %>
表达式可以是任何完整的java表达式。结果被转为字符串
```

## 注释

```
<%-- 注释 --%>
该注释方法在显示时是隐藏的

对代码片断的注释
<!-- <%= new Date() %>  -->

动态注释
<!-- <%=new Date() %> -->

 /**    说明内容
   */
在方法或变量上加入/**注释可以被javadoc获取，当鼠标移动到该方法或变量时会提示注释内容
```

## 声明标识

```
<%!    全局变量或方法   %>
服务器执行JSP页面时，会将JSP页面转换为Servlet类，把页面中定义的全局变量和方法转换为类的成员变量和方法。

声明的标识一直有效，当前页面有效，服务器关闭时销毁
```
