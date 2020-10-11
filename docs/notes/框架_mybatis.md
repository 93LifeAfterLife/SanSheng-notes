### Log4j

#### 常用配置:

```properties
log4j.rootLogger=INFO, Console, File
log4j.logger.com.td=DEBUG

# Console
log4j.appender.Console=org.apache.log4j.ConsoleAppender
log4j.appender.Console.layout=org.apache.log4j.PatternLayout
log4j.appender.Console.layout.ConversionPattern=%d [%-5p] %c - %m%n

# File
log4j.appender.File=org.apache.log4j.FileAppender
log4j.appender.File.File=E:/WorkSpaces/TestWS/SS-MYBATIS-V1.01/src/main/resources/log/debug.log
log4j.appender.File.layout=org.apache.log4j.PatternLayout
log4j.appender.File.layout.ConversionPattern=%d [%-5p] %c - %m%n
```

