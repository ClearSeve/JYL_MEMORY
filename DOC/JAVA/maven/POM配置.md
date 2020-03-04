# POM配置

## jdk8支持

```xml
<properties>
  <maven.compiler.source>1.8</maven.compiler.source>
  <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```

## 导出jar包

+ 添加删除插件

```xml
<build>
  <pluginManagement>
     <plugins>
位置下加入插件：


<plugin>
 <groupId>org.apache.maven.plugins</groupId>
 <artifactId>maven-shade-plugin</artifactId>
 <version>2.3</version>
 <executions>
   <execution>
     <phase>package</phase>
     <goals>
       <goal>shade</goal>
     </goals>
     <configuration>
       <transformers>
         <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
           <mainClass>com.jyl.App</mainClass>
         </transformer>
       </transformers>
     </configuration>
   </execution>
 </executions>
</plugin>


删除插件

<plugin>
  <artifactId>maven-jar-plugin</artifactId>
  <version>3.0.2</version>
</plugin>
```

+ 添加声明

```xml
</pluginManagement>
结尾加入


<plugins>
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-shade-plugin</artifactId>
  </plugin>
</plugins>
```
