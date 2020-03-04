# nexus

## nexus配置

```
https://www.sonatype.com/download-oss-sonatype

参数修改：
修改数据存储路径，文件目录：bin/nexus.vmoptions
修改IP、端口、访问根目录，文件目录：etc/nexus-default.properties
```

## 启动

```
运行bin目录下的exe
./nexus.exe /run

默认端口8081
http://127.0.0.1:8081

使用命令可以安装服务：
nexus.exe /install
```

## 仓储设置

```
使用管理员账号登录，默认登录名及密码：admin  admin123


maven-central：maven中央库，默认从https://repo1.maven.org/maven2/拉取jar 
maven-releases：私库发行版jar 
maven-snapshots：私库快照（调试版本）jar 
maven-public：仓库分组，把上面三个仓库组合在一起对外提供服务，在本地maven基础配置settings.xml中使用。
```

## maven配置

```
settings.xml文件内容：

<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <localRepository>C:\JaveWeb\mvn</localRepository>

  <pluginGroups>
  <pluginGroup>org.sonatype.plugins</pluginGroup>
  </pluginGroups>

  <proxies>
  </proxies>


  <servers>
    <server>
      <id>nexus</id>
      <username>admin</username>
      <password>admin123</password>
    </server>
  </servers>


  <mirrors>
  <mirror>
      <id>nexus</id>
      <mirrorOf>*</mirrorOf>
      <url>http://localhost:8081/repository/maven-public/</url>
    </mirror>
  </mirrors>

  <profiles>
  <profile>
      <id>nexus</id>
      <repositories>
        <repository>
          <id>central</id>
          <url>http://central</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>true</enabled></snapshots>
        </repository>
      </repositories>
     <pluginRepositories>
        <pluginRepository>
          <id>central</id>
          <url>http://central</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>true</enabled></snapshots>
        </pluginRepository>
      </pluginRepositories>
    </profile>
  </profiles>

  <activeProfiles>
    <activeProfile>nexus</activeProfile>
  </activeProfiles>
</settings>
```

## 工程配置pox.xml

```
  <distributionManagement>
  <repository>
      <id>nexus</id>
      <name>Releases</name>
      <url>http://localhost:8081/repository/maven-releases</url>
    </repository>
    <snapshotRepository>
      <id>nexus</id>
      <name>Snapshot</name>
      <url>http://localhost:8081/repository/maven-snapshots</url>
    </snapshotRepository>
  </distributionManagement>
  <build>
    <defaultGoal>compile</defaultGoal>
    <finalName>page</finalName>
    <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.4.2</version>
          <configuration>
            <skipTests>true</skipTests>
          </configuration>
        </plugin>
        <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
    </plugins>
  </build>
```

## 编译项目

```
项目右单击->Run As->Maven build..
在界面中填写Goals为：deploy -e
POM配置为<version>0.0.1-SNAPSHOT</version>，则编译到私服的maven-snapshots里。<version>0.0.1</version>则编译到maven-release里。
```

## 上传jar包

```
mvn deploy:deploy-file
  -DgroupId=com.aliyun.oss   -DartifactId=aliyun-sdk-oss   -Dversion=2.2.3   -Dpackaging=jar   -Dfile=D:\aliyun-sdk-oss-2.2.3.jar   -Durl=http://127.0.0.1:8081/repository/maven-3rd/   -DrepositoryId=nexus-releases
```
