# Servlet

## 创建

```
实现 Servlet 接口或者继承 HttpServlet 方法
init方法和destroy方法为Servlet初始化与生命周期结束所调用。doGet和doPost方法用于http的get与post请求。

=========创建serverlet类
创建com.test.servletTe

package com.test;
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class servletTe extends HttpServlet {
    private static final long serialVersionUID = 1L;
     private String message;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public servletTe() {
        super();
        message = "servletTe";
    }
    /**
     * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
     */
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

      response.setContentType("text/html;charset=utf-8");
      response.setCharacterEncoding("utf-8");
      PrintWriter out = response.getWriter();
      String str = "我是网页内容";
      out.write(str);
    }
    /**
     * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
     */
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // TODO Auto-generated method stub
        doGet(request, response);
    }

}


=========调用方式1
web.xml中加入以下配置：
<servlet>
    <servlet-name>servletTe</servlet-name>
    <servlet-class>com.test.servletTe</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletTe</servlet-name>
    <url-pattern>/servletTe</url-pattern>
</servlet-mapping>

url访问/servletTe时调用serverlet

=========调用方式2
直接在类上加入注解
@WebServlet("/servletTe")
或者@WebServlet(name="servletTe",urlPatterns="/servletTe")
```

## 过滤器

```
Servlet过滤器实现Filter接口，可以配置对指定页面进行过滤，当请求指定过滤页面时先调用过滤器的doFilter

===========访问页面计数，每次请求页面时计数增加1==========
===========配置  
<filter>
       <filter-name>SF</filter-name>
       <filter-class>com.WebTest.SF</filter-class>
       <init-param>
          <param-name>count</param-name>
          <param-value>10</param-value>
       </init-param>
 </filter>
 <filter-mapping>
       <filter-name>SF</filter-name>
       <url-pattern>/index.jsp</url-pattern>
</filter-mapping>
在后面继续配置第二个<filter>同时定位到同一个<url-pattern>，可以做过滤链。


@WebFilter("/*")                  指定所有页面都使用
@WebFilter("/index.jsp")          指定index.jsp使用过滤

import javax.servlet.annotation.WebInitParam;
@WebFilter(urlPatterns = "/index.jsp",initParams= {@WebInitParam(name="aa",value="a")})   
 指定index.jsp使用过滤，设置初始参数aa

===========类定义
包含count成员变量

init函数中使用fConfig对象获取web.xml中<init-param>中配置的参数
String Count = fConfig.getInitParameter("count");
if(null == Count) return;
count = Integer.valueOf(count);


doFilter函数中进行计数，并写入到application对象
++count;
HttpServletRequest req = (HttpServletRequest)request;
ServletContext context = req.getSession().getServletContext();
context.setAttribute("count", count);

===========index.jsp中获取参数
<%=application.getAttribute("count")%>
```

## 监听器

```
实现监听接口HttpSessionBindingListener
public class SL implements HttpSessionBindingListener



java文件中注解@WebListener加入绑定
或者web.xml加入配置
  <listener>
    <listener-class>com.WebTest.SL</listener-class>
  </listener>

注解中可以加入描述
@WebListener("description")

在jsp页面中调用session对象的setAttribute/removeAttribute会触发SL的valueBound/valueUnbound方法
session.setAttribute("name", new SL());  
session.removeAttribute("name");
```

## 监听器上传文件

```
@MultipartConfig(location="D:/")  //临时目录必须有访问权限

======上传界面
<form action="SL" enctype="multipart/form-data" method="post">
  <br/>
  <input type="file" name="f1"/>
  <br />
  <input type="submit" name="upload" value="upload" />
</form>

====== SL 类的doPost
response.setContentType("text/html;charset=UTF-8");
PrintWriter out = response.getWriter();
String path = this.getServletContext().getRealPath("/");
javax.servlet.http.Part p = request.getPart("f1");
if(p.getContentType().contains("image"))//只处理image文件
{
    ApplicationPart ap = (ApplicationPart)p;
    String PathFileName = ap.getSubmittedFileName();
    int path_idx = PathFileName.lastIndexOf("\\") + 1;
    String fileName = PathFileName.substring(path_idx,PathFileName.length());
    p.write("D:/workspace/" + fileName); //存储目录必须存在
    out.write("suc");
}
else
{
  out.write("failed");
}
```
