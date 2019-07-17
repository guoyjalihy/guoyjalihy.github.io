---
layout: post
title:  "SpringBoot的日志详解"
date:   2018-12-16 18:41:01 +0800
categories: springboot
---
### 1、简述
SpringBoot官方文档关于日志的整体说明

<figure>
    <img src="{{ site.baseurl }}/images/springboot-log.jpg" alt="image">
    <figcaption>
        springboot-log
    </figcaption>
</figure>
SpringBoot 内部日志系统使用的是 Commons Logging 并且 SpringBoot 给 JDKLogging , Log4j2(Log4j也是支持的) , Logback 都提供了默认配置，并且如果使用了 Starters ，那么默认使用 Logback 。

```properties

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.xiaoye</groupId>
    <artifactId>demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>demo</name>
    <description>SpringBootDemo</description>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <!-- 
            Spring Boot应用启动器Starter-parent:
            官方推荐
            其中集成了：
            1、使用java6编译级别
            2、使用utf-8编码
            3、实现了通用的测试框架 (JUnit, Hamcrest, Mockito).
            4、智能资源过滤
            5、智能的插件配置(exec plugin, surefire, Git commit ID, shade).
        -->
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.3.RELEASE</version>
        <relativePath />
    </parent>
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <dependency>
            <!-- 
            Spring Boot应用启动器Starter-web:
            支持全栈式Web开发，包括Tomcat和spring-webmvc
             -->
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
</project>

```
简单的说，只要你的 pom 文件中使用了 spring-boot-starter 就代表着你使用了 SpringBoot 的 Starters 。相信各位玩 SpringBoot 的朋友肯定是看到了自己的 pom 文件中有这个了，因为，Starters（启动器）是 SpringBoot 最核心的部件之一，没有了启动器， SpringBoot 就几乎废掉了。

### 2、使用
#### 2.1 log level
SpringBoot 招人喜欢的一大特点就是配置方便，配置日志的相关参数也只需要写在 application.properties 中就可以了，当然，这仅仅是基本的配置，如果需要高级的配置，还是需要添加依赖所选择日志系统的配置文件。

<figure>
    <img src="{{ site.baseurl }}/images/springboot-log-level.png" alt="image">
    <figcaption>
        springboot-log-level
    </figcaption>
</figure>

SpringBoot官方文档关于配置Log级别的说明

官方文档中有提到， SpringBoot 的 Logging 配置的级别有7个：  
TRACE , DEBUG , INFO , WARN , ERROR , FATAL , OFF  

配置格式：  
logging.level.*=LEVEL  

举例：  
```properties

    #root日志以WARN级别输出
    logging.level.root=WARN
    #springframework.web日志以DEBUG级别输出
    logging.level.org.springframework.web=DEBUG
    #hibernate日志以ERROR级别输出
    logging.level.org.hibernate=ERROR
```

在进行了这样的配置后，就可以在控制台打印Log信息了！

但是很有人又会问了，在生产环境中，日志往往要以文件形式存放到本地，那么 SpringBoot 的默认配置文件能够实现吗？答案是可以的，我们继续看官方文档：

<figure>
    <img src="{{ site.baseurl }}/images/srping-log-file.png" alt="image">
    <figcaption>
        srping-log-file
    </figcaption>
</figure>

文档中对 SpringBoot 日志的文件输出有明确的说明，上面说到：  
在默认情况下，SpringBoot 是仅仅在控制台打印log信息的，如果我们需要将log信息记录到文件，那么就需要在 application.properties 中配置 logging.file 或者 logging.path （注意啊，是或者，不是并且！）
而且官方文档有明确说明，配置 logging.file 的话是可以定位到自定义的文件的，使用 logging.path 的话，日志文件将使用 spring.log 来命名。
而且日志文件会在10Mb大小的时候被截断，产生新的日志文件，默认级别为：ERROR、WARN、INFO

这时候又会有很多同学问了，那如果我要自定义输出格式怎么办呢？其实在 application.properties 也是可以办到的。继续看官方文档：


<figure>
    <img src="{{ site.baseurl }}/images/springboot-log-pattern.png" alt="image">
    <figcaption>
        springboot-log-pattern
    </figcaption>
</figure>

SpringBoot官方文档关于日志配置的说明


但是要注意：这两个配置项是只对默认的日志系统Logback起作用的

举例（application.properties）：
```properties
logging.level.root=WARN
logging.level.org.springframework.web=DEBUG
logging.file=/log/log/my.log
logging.pattern.console=%d{yyyy/MM/dd-HH:mm:ss} [%thread] %-5level %logger- %msg%n
logging.pattern.file=%d{yyyy/MM/dd-HH:mm} [%thread] %-5level %logger- %msg%n
```

```cmd
2018/12/16-17:36:06 [main] DEBUG org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping- Rejected bean name 'systemEnvironment': no URL paths identified
2018/12/16-17:36:06 [main] DEBUG org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping- Rejected bean name 'org.springframework.context.annotation.ConfigurationClassPostProcessor.importRegistry': no URL paths identified
2018/12/16-17:36:06 [main] DEBUG org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping- Rejected bean name 'messageSource': no URL paths identified
2018/12/16-17:36:06 [main] DEBUG org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping- Rejected bean name 'servletContext': no URL paths identified
2018/12/16-17:36:06 [main] DEBUG org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping- Rejected bean name 'contextParameters': no URL paths identified
2018/12/16-17:36:06 [main] DEBUG org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping- Rejected bean name 'contextAttributes': no URL paths identified
2018/12/16-17:36:06 [main] INFO  org.springframework.web.servlet.handler.SimpleUrlHandlerMapping- Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018/12/16-17:36:06 [main] INFO  org.springframework.web.servlet.handler.SimpleUrlHandlerMapping- Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018/12/16-17:36:06 [main] DEBUG org.springframework.web.servlet.mvc.method.annotation.ExceptionHandlerExceptionResolver- Looking for exception mappings: org.springframework.boot.context.embedded.AnnotationConfigEmbeddedWebApplicationContext@15ef3ab: startup date [Thu Sep 22 17:36:04 GMT+08:00 2016]; root of context hierarchy
2018/12/16-17:36:06 [main] INFO  org.springframework.web.servlet.handler.SimpleUrlHandlerMapping- Mapped URL path [/**/favicon.ico] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018/12/16-17:36:06 [main] DEBUG org.springframework.web.servlet.resource.ResourceUrlProvider- Looking for resource handler mappings
2018/12/16-17:36:06 [main] DEBUG org.springframework.web.servlet.resource.ResourceUrlProvider- Found resource handler mapping: URL pattern="/**/favicon.ico", locations=[ServletContext resource [/], class path resource [META-INF/resources/], class path resource [resources/], class path resource [static/], class path resource [public/], class path resource []], resolvers=[org.springframework.web.servlet.resource.PathResourceResolver@b1df4d]
2018/12/16-17:36:06 [main] DEBUG org.springframework.web.servlet.resource.ResourceUrlProvider- Found resource handler mapping: URL pattern="/webjars/**", locations=[class path resource [META-INF/resources/webjars/]], resolvers=[org.springframework.web.servlet.resource.PathResourceResolver@164e13b]
2018/12/16-17:36:06 [main] DEBUG org.springframework.web.servlet.resource.ResourceUrlProvider- Found resource handler mapping: URL pattern="/**", locations=[ServletContext resource [/], class path resource [META-INF/resources/], class path resource [resources/], class path resource [static/], class path resource [public/]], resolvers=[org.springframework.web.servlet.resource.PathResourceResolver@1fefcb1]
2018/12/16-17:36:06 [main] DEBUG org.springframework.web.context.support.StandardServletEnvironment- Adding [server.ports] PropertySource with highest search precedence
```
好了，这时候大家已经对 application.properties 中配置Log有了了解，但是这时候又会发现一个问题， application.properties 中的配置有的时候不能满足我们的要求，或者我们要使用其他的集成进SpringBoot的日志系统，该怎么办呢？官方文档又给了我们答案：

<figure>
    <img src="{{ site.baseurl }}/images/springboot-log-more.png" alt="image">
    <figcaption>
        springboot-log-more
    </figcaption>
</figure>

SpringBoot官方文档关于使用指定日志配置的说明


我们能从中知道:  
1.通过将适当的库添加到classpath，可以激活各种日志系统。然后在 classpath 的根目录 （root） 或通过 Spring Environment（ application.properties）的 logging.config 属性指定的位置提供一个合适的配置文件来达到进一步的定制（注意由于日志是在ApplicationContext被创建之前初始化的，所以不可能在Spring的@Configuration文件中，通过@PropertySources控制日志。系统属性和平常的Spring Boot外部配置文件能正常工作）。  
2.如果我们使用指定日志系统的配置文件， application.properties 中相关的日志配置是可以不要的。  
3.支持的三种日志系统（ Logback , Log4j2 （ Log4j 也是支持的）, JDKLogging ）所识别的配置文件名。  

好了，接下来我们就要学习如何在 SpringBoot 中玩这些指定的日志系统了。

### 3、扩展
#### 3.1使用Logback的指定配置文件实现更高级的日志配置
如果我们的确要使用 Logback 的指定配置文件的话，那么说明 application.properties 中的配置功能已经不满足我们需求了，所以 application.properties 中关于日志记录的可以不要了，因为我们启用了 Logback 自己的配置文件，启用的方式很简单，在 classpath 的 resources 下新建 logback.xml 文件。这样就可以了。  
具体配置和说明这里有个简单的介绍：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <!-- 控制台打印日志的相关配置 --> 
  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender"> 
    <!-- 日志格式 -->
    <encoder>
        <pattern>%d{yyyy-MM-dd HH:mm:ss} [%level] - %m%n</pattern>
    </encoder>
    <!-- 日志级别过滤器 -->
    <filter class="ch.qos.logback.classic.filter.LevelFilter">
      <!-- 过滤的级别 -->
      <level>WARN</level>
      <!-- 匹配时的操作：接收（记录） -->
      <onMatch>ACCEPT</onMatch>
      <!-- 不匹配时的操作：拒绝（不记录） -->
      <onMismatch>DENY</onMismatch>
    </filter>
  </appender>

  <!-- 文件保存日志的相关配置 --> 
  <appender name="ERROR-OUT" class="ch.qos.logback.core.rolling.RollingFileAppender">
     <!-- 保存日志文件的路径 -->
    <file>/logs/error.log</file>
    <!-- 日志格式 -->
    <encoder>
        <pattern>%d{yyyy-MM-dd HH:mm:ss} [%class:%line] - %m%n</pattern>
    </encoder>
    <!-- 日志级别过滤器 -->
    <filter class="ch.qos.logback.classic.filter.LevelFilter">
      <!-- 过滤的级别 -->
      <level>ERROR</level>
      <!-- 匹配时的操作：接收（记录） -->
      <onMatch>ACCEPT</onMatch>
      <!-- 不匹配时的操作：拒绝（不记录） -->
      <onMismatch>DENY</onMismatch>
    </filter>
    <!-- 循环政策：基于时间创建日志文件 -->
    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
      <!-- 日志文件名格式 -->
      <fileNamePattern>error.%d{yyyy-MM-dd}.log</fileNamePattern>
      <!-- 最大保存时间：30天-->
      <maxHistory>30</maxHistory>
    </rollingPolicy>
  </appender>

  <!-- 基于dubug处理日志：具体控制台或者文件对日志级别的处理还要看所在appender配置的filter，如果没有配置filter，则使用root配置 -->
  <root level="debug">
    <appender-ref ref="STDOUT" />
    <appender-ref ref="ERROR-OUT" />
  </root>
</configuration>
```
#### 3.2使用Log4j的指定配置文件实现更高级的日志配置
1.更改pom文件：  
在创建 SpringBoot 工程时，我们引入了 spring-boot-starter，其中包含了 spring-boot-starter-logging ，该依赖内容就是 SpringBoot 默认的日志框架 Logback ，所以我们在引入 log4j 之前，需要先排除该包的依赖，再引入 log4j 的依赖。
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j</artifactId>
</dependency>
```
之后我们可以使用 application.properties，其中的配置项之前有说，同样也是适用 Log4J 的。  
如果需要更高级的配置选择，必须要添加 Log4j 的配置文件了。我们在 classpath 的 resources下新建 log4j.properties 文件，这里简单示例一下：

```properties
# 日志级别，日志追加程序列表...
log4j.rootLogger=DEBUG,ServerDailyRollingFile,stdout
#文件保存日志
log4j.appender.ServerDailyRollingFile=org.apache.log4j.DailyRollingFileAppender
#文件保存日志日期格式
log4j.appender.ServerDailyRollingFile.DatePattern='.'yyyy-MM-dd_HH
#文件保存日志文件路径
log4j.appender.ServerDailyRollingFile.File=/mnt/lunqi/demo/log4j.log
#文件保存日志布局程序
log4j.appender.ServerDailyRollingFile.layout=org.apache.log4j.PatternLayout
#文件保存日志布局格式
log4j.appender.ServerDailyRollingFile.layout.ConversionPattern=%d - %m%n
#文件保存日志需要向后追加（false是测试的时候日志文件就清空，true的话就是在之前基础上往后写）
log4j.appender.ServerDailyRollingFile.Append=false
#控制台日志
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
#控制台日志布局程序
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
#控制台日志布局格式
log4j.appender.stdout.layout.ConversionPattern=%d yyyy-MM-dd HH:mm:ss %p [%c] %m%n
```

#### 3.3使用Log4j2的指定配置文件实现更高级的日志配置
1.更改pom文件：  
跟引入Log4j一样，我们也需要排除 spring-boot-starter-logging ，再引入Log4j2的依赖:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```
2.跟Log4j一样，我们可以使用 application.properties ，如果需要更高级的配置选择，必须要添加 Log4j2 的配置文件了。我们在 classpath 的 resources下新建 log4j2.xml 文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration> 
    <appenders> 
        <Console name="Console" target="SYSTEM_OUT"> 
            <ThresholdFilter level="trace" onMatch="ACCEPT" onMismatch="DENY" /> 
            <PatternLayout pattern="%d{HH:mm:ss.SSS} %-5level %class{36} %L %M - %msg%xEx%n" />
        </Console>
        <File name="log" fileName="log/test.log" append="false">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} %-5level %class{36} %L %M - %msg%xEx%n" />
        </File> 
        <RollingFile name="RollingFile" fileName="logs/spring.log" filePattern="log/$${date:yyyy-MM}/app-%d{MM-dd-yyyy}-%i.log">
            <PatternLayout pattern="%d{yyyy-MM-dd 'at' HH:mm:ss z} %-5level %class{36} %L %M - %msg%xEx%n" />
            <SizeBasedTriggeringPolicy size="50MB" />
        </RollingFile>
    </appenders> 

    <loggers> 
        <root level="trace">
            <appender-ref ref="RollingFile" />
            <appender-ref ref="Console" />
        </root>
    </loggers>
</configuration>
```
这里所有的配置信息和配置项在说Logback和Log4j的时候都有说过，所以Log4j2的配置文件肯定是能看懂的。