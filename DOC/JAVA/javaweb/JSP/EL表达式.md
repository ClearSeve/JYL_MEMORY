# EL表达式

```
开启el表达式（默认可能是关闭的）
<%@page isELIgnored="false" %>


${***}
等价于<%=request.getAttribute("***")%>


jsp页面中输出$符号时:\${  或者  ${"${"}
可以访问pageContext对象
除0后返回Infinity,不返回错误
```

## 禁用

```
禁用后将原内容输出

禁用一个表达式:\${**}
禁用所有页面(web.xml):
<jsp-config>
    <jsp-property-group>
      <url-pattern>*.jsp</url-pattern>
      <el-ignored>true</el-ignored>
   </jsp-property-group>
</jsp-config>
```

## 判空

```
判断对象是否是null或者””
<% request.setAttribute("nn", ""); %>
${empty nn}
```

## 访问作用范围的隐含对象

```
<% request.setAttribute("nn", ""); %>
需要使用requestScope.nn
不能通过request.nn访问到保存到request范围内的变量nn


根据设置的访问范围进行获取
<jsp:useBean id="tt" scope="page" class="com.wt.tt"></jsp:useBean>
${ pageScope.tt.val}
```

## 访问环境中的值

```
获取提交的标单中txt变量的值
${param.txt}


获取提交的标单中一组值
${paramValues.ck[0]}
${paramValues.ck[1]}
```
