# mybatis调用

## 依赖

```
<dependency>
	<groupId>org.mybatis</groupId>
	<artifactId>mybatis</artifactId>
	<version>3.4.6</version>
</dependency>
<dependency>
   <groupId>mysql</groupId>
   <artifactId>mysql-connector-java</artifactId>
   <version>8.0.11</version>
</dependency>
```

## 实体类

创建包com.test.domain

```
package com.test.domain;

public class User {
	private String name;	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}	
}
```

## mapper

创建包com.test.mapper

### userMapper接口

```
package com.test.mapper;
import com.test.domain.User;

public interface userMapper {
	public User fun(int id) throws Exception;
}
```

### userMapper.xml

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.test.mapper.userMapper">
    <select id="fun" parameterType="int" 
        resultType="com.test.domain.User">
        select name from user where id=#{id}
    </select>
</mapper>
```

## cfg.xml
将配置放在com.test.mapper
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
     <environments default="development">
         <environment id="development">
             <transactionManager type="JDBC" />
             <dataSource type="POOLED">
                 <property name="driver" value="com.mysql.jdbc.Driver" />
                 <property name="url" value="jdbc:mysql://localhost:3306/testdb?useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=GMT%2B8" />
                 <property name="username" value="root" />
                 <property name="password" value="root" />
             </dataSource>
         </environment>
     </environments>
  <mappers>
     <package name="com/test/mapper"/>
  </mappers>
</configuration>
```

告知映射文件方式如果使用：
```
<mappers>
    <mapper resource="com/test/mapper/userMapper.xml"/>
</mappers>
```
则可以不创建userMapper的接口类

## 调用

```
import java.io.IOException;
import java.io.Reader;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import com.test.domain.User;

public class App{

	public static void main(String[] args) throws IOException {
        Reader reader = Resources.getResourceAsReader("com/test/mapper/cfg.xml");
		SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(reader);
	    SqlSession session = sessionFactory.openSession();

 	          
//      String statement = "com.test.mapper.userMapper.fun";
//      int id = 0;
// 	    User user = session.selectOne(statement, id);
//      session.commit();
//      System.out.println(user.getName());
         
         int idParam = 0;
         userMapper mapper = session.getMapper(userMapper.class);
         User userRet =  mapper.fun(idParam);
         System.out.println(userRet.getName());
	}
}
```
