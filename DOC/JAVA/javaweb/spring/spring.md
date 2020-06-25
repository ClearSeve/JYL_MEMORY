# spring

```xml
<dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-context</artifactId>
   <version>4.0.3.RELEASE</version>
  </dependency>
  <dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-web</artifactId>
   <version>4.0.3.RELEASE</version>
  </dependency>
  <dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-webmvc</artifactId>
   <version>4.0.3.RELEASE</version>
</dependency>
```

## IoC

反向控制IoC（Inversion of Control），指实现必须依赖抽象，而不是抽象依赖实现。  
依赖注入DI（Dependency Injection），用来实现反向控制功能。包含三种实现方式：接口注入、Set注入、构造注入。  spring实现Set注入和构造注入。

## javabean类的编写

```
定义一个接口（通过接口进行调用）
public interface AA {
   void show();
}


编写实现类AAa
public class AAa implements AA{

   public void show() {
      System.out.println(msg);
      System.out.println(val);
      System.out.println(val2);
   }  
   String val;
   String val2;
   public AAa(String co1,String co2)
   {
      val = co1;
      val2 = co2;
      return msg;
   }
   public void setMsg(String msg) {
      this.msg = msg;
   }
}


编写实现类AAb
public class AAb implements AA {
   public void show() {
      System.out.println(date);
   }  
   Date date = null;
   public Date getDate() {
      return date;
   }
   public void setDate(Date date) {
      this.date = date;
   }
}

```

## xml编写

```
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd">

  <!--  
  class包含类的全路径，通过property使用SetXX方法设置成员变量msg
  通过constructor-arg进行构造注入
  -->
  <bean id="idName1" class="inf.AAa" >
    <constructor-arg>
       <value>arg1</value>
    </constructor-arg>
    <constructor-arg>
       <value>arg2</value>
    </constructor-arg>
    <property name="msg">
      <value>xxx</value>
    </property>
  </bean>
  
  <!--  
  Set注入方式设置AAb类中的date对象
  可以使用depends-on强制初始化依赖的类date
  -->
  <bean id="idName2" class="inf.AAb" depends-on="date">
    <property name="date">
      <ref bean="date"/>
    </property>
  </bean>
  <bean id="date" class="java.util.Date"></bean>
  
</beans>
```

## 调用

```
import  org.springframework.context.support.FileSystemXmlApplicationContext;


FileSystemXmlApplicationContext ctx = new FileSystemXmlApplicationContext("setting.xml");
//xml放在项目根目录

AA a = (AA)ctx.getBean("idName1");
a.show();

AA b = (AA)ctx.getBean("idName2");
b.show();
```
