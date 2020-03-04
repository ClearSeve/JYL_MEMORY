# IDE

## IDEA配置导出war

```
Mave Projects -> Lifecycle -> package

<build>
 <plugins>
   <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-shade-plugin</artifactId>
    <version>1.2.1</version>
    <executions>
     <execution>
      <phase>package</phase>
      <goals>
        <goal>shade</goal>
      </goals>
      <configuration>
       <transformers>
        <transformer
          implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
          <mainClass>com.test.MainClass</mainClass>
        </transformer>
       </transformers>
      </configuration>
     </execution>
    </executions>
   </plugin>
 </plugins>
</build>


<properties>
  <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  <maven.compiler.source>1.8</maven.compiler.source>
  <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```

## eclipse导出运行

```
右键工程-》Run As -》Maven install

项目的target目录中运行：
java -jar  ******-0.0.1-SNAPSHOT.jar
```
