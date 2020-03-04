# javaBean

```
javaBean类需要遵守规范：
提供一个默认的无参构造函数。
可能有一系列可读写属性,首字母必须小写
可能有一系列的"getter"或"setter"方法。


=====创建bean类
package com.test;
public class bb {
   private String val = null;
   public String getVal() {
      return val;
   }  
   public void setVal(String val) {
      this.val = val;
   }
}

=====使用
<%--创建对象objID --%>
<jsp:useBean id="ObjID" class="com.test.bb"/>

<%--设置对象objID的val属性 --%>
<jsp:setProperty name="ObjID"  property="val" value="aaabbbccc"/>

<%--显示 --%>
val = <jsp:getProperty name="ObjID"  property="val"/>
```
