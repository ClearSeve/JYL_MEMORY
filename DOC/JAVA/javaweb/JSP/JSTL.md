# JSTL

## 依赖

```xml
<dependency>
       <groupId>javax.servlet</groupId>
       <artifactId>jstl</artifactId>
       <version>1.2</version>
    </dependency>
    <dependency>
       <groupId>taglibs</groupId>
       <artifactId>standard</artifactId>
       <version>1.1.2</version>
</dependency>
```

## 使用

```
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

+ 获取运行路径

```
<c:set var="pageurl" value="${pageContext.request.contextPath}"></c:set>
使用：
'${pageurl}/resources/icons/x.png'
```

+ 输出字符串

```
<c:out value="xxx"/>
<c:out value="${aaa}"/>
<c:out value="${aaa}" default="xxx"/>
```

+ 在pageContext中添加值
`<c:set var="aaa" value="xxx"/>`

+ 在session中添加值
`<c:set var="a" value="hello" scope="session"/>`

+ 删除pageContext中数据 无scope时删除所有域中的数据
`<c:remove var="a" scope="page"/>`

+ if语句

```
<c:if test="${booleanVal}"> **** </c:if>
<c:if test="${strval.equals('ss')}"> **** </c:if>  
```

+ 选择结构

```
<c:set var="score" value="${param.score }"/>  
<c:choose>  
    <c:when test="${score > 100 || score < 0}">错误的分数：${score }</c:when>  
    <c:when test="${score >= 90 }">A级</c:when>  
    <c:when test="${score >= 80 }">B级</c:when>  
    <c:when test="${score >= 70 }">C级</c:when>  
    <c:when test="${score >= 60 }">D级</c:when>  
    <c:otherwise>E级</c:otherwise>  
</c:choose>  
```

+ for循环

```
for(i =0 ; i < 10;++i)

<c:forEach var="i" begin="1" end="10">
    ...
</c:forEach>  
```

+ 遍历

```
for(obj : listObj) var = obj.imageurl

需要强转url为字符串时：
src="'+url+'"

<c:forEach items="${listObj}" var="obj>  
   <img src="${pageurl }/resources/image/${obj.imageurl }" /> 
</c:forEach>  
```

## 自定义标签

### Java类

```
package com.test.domain;

import java.io.IOException;

import javax.servlet.jsp.JspWriter;
import javax.servlet.jsp.PageContext;
import javax.servlet.jsp.tagext.SimpleTagSupport;

public class TeTag extends SimpleTagSupport {

   private String strParam;
   private int    intParam;

   public String getStrParam() {
      return strParam;
   }
   public void setStrParam(String strParam) {
      this.strParam = strParam;
   }
   public int getIntParam() {
      return intParam;
   }
   public void setIntParam(int intParam) {
      this.intParam = intParam;
   }  


    @Override
    public void doTag() throws IOException
    {
        PageContext pc = (PageContext)getJspContext();
        JspWriter out = pc.getOut();
        out.print(strParam + "=====" + intParam);
}



public static String funEL(String val){
   return val + "===EL===";
}
}
```

### tld文件

```
目录：/WEB-INFO/TeTag.tld

<?xml version="1.0" encoding="UTF-8" ?>

<taglib xmlns="http://java.sun.com/xml/ns/j2ee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-jsptaglibrary_2_0.xsd"
  version="2.0">

  <tlib-version>1.1</tlib-version>
  <short-name>ssname</short-name>
  <uri>*******</uri>
  
  
  <tag>
  <name>fun</name>
  <tag-class>com.test.domain.TeTag</tag-class>
  <body-content>empty</body-content>
  <attribute>
  <name>strParam</name>
  <required>true</required>
  <rtexprvalue>true</rtexprvalue>
  </attribute>
  <attribute>
  <name>intParam</name>
  <required>true</required>
  <rtexprvalue>true</rtexprvalue>
  </attribute>
  </tag>




<function>
   <name>funEL</name>
   <function-class>com.test.domain.TeTag</function-class>
<function-signature>java.lang.String funEL(java.lang.String)</function-signature>
</function>

</taglib>
```

### Jsp页面

```
<%@ taglib prefix="Te" uri="/WEB-INF/TeTag.tld" %> 
<Te:fun strParam="aaaa" intParam="15"/>


<%@page isELIgnored="false" %>
${Te:funEL('abcd')}
```
