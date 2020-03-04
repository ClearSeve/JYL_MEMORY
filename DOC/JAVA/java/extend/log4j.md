# log4j

## 配置说明

```
log4j.rootLogger = [ level ] , appenderName, appenderName, …

level 是日志记录的优先级
优先级从高到低：【字母不区分大小写】
OFF、FATAL、ERROR、WARN、INFO、DEBUG、ALL、自定义的级别。
通过在这里定义level，可以控制到应用程序中相应级别的日志信息的开关。比这里定义了INFO级别，则应用程序中所有INFO级别以下日志信息将不被输出。


appenderName指带后面要配置的日志控制，可以同时编写多个日志控制。其中的Threshold字段定义了控制级别，表示控制该级别以上的日志。
如：log4j.appender.****.Threshold = DEBUG 表示控制DEBUG 级别以上的日志的输出位置（在原有底层优先级日志输出的基础上，将DEBUG 级别以上的日志再输出到另一个文件）
配置日志信息输出目的地Appender，其语法为：
log4j.appender.appenderName = fully.qualified.name.of.appender.class  
log4j.appender.appenderName.option1 = value1  
log4j.appender.appenderName.option = valueN
其中，Log4j提供的appender有以下几种：
org.apache.log4j.ConsoleAppender（控制台），  
org.apache.log4j.FileAppender（文件），  
org.apache.log4j.DailyRollingFileAppender（每天产生一个日志文件），  
org.apache.log4j.RollingFileAppender（文件大小到达指定尺寸的时候产生一个新的文件），  
org.apache.log4j.WriterAppender（将日志信息以流格式发送到任意指定的地方）

```

## 依赖

```xml
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

## 配置文件

```
log4j.properties文件放在工程生成的classpath下
java文件夹下放java代码，该目录中的文件就会放在classpath下
javaweb中放在WEB-INF/classes

### 设置###
log4j.rootLogger = debug,A,B,C

### 输出ALL级别以上日志到控制台###
log4j.appender.A = org.apache.log4j.ConsoleAppender
log4j.appender.A.Target = System.out
log4j.appender.A.layout = org.apache.log4j.PatternLayout
log4j.appender.A.layout.ConversionPattern = [%-5p] %d{yyyy-MM-dd HH:mm:ss,SSS} method:%l%n%m%n


### 输出DEBUG 级别以上的日志到=./logs/a.log  javaweb只能绝对路径###
log4j.appender.B = org.apache.log4j.DailyRollingFileAppender
log4j.appender.B.File = ./logs/a.log
log4j.appender.B.Append = true
log4j.appender.B.Threshold = DEBUG 
log4j.appender.B.layout = org.apache.log4j.PatternLayout
log4j.appender.B.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss}  [ %t:%r ] - [ %p ]  %m%n

### 输出ERROR 级别以上的日志到=./logs/b.log ###
log4j.appender.C = org.apache.log4j.DailyRollingFileAppender
log4j.appender.C.File =./logs/b.log 
log4j.appender.C.Append = true
log4j.appender.C.Threshold = ERROR 
log4j.appender.C.layout = org.apache.log4j.PatternLayout
log4j.appender.C.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss}  [ %t:%r ] - [ %p ]  %m%n
```

## 调用

```
import org.apache.log4j.Logger;

private static Logger logger = Logger.getLogger(类名.class); 

logger.debug("debug");  
logger.info("info");  
logger.warn("warning");
logger.error("error");  
logger.fatal("fatal");
```
