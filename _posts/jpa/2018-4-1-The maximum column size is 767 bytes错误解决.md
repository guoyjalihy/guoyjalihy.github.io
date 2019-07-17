---
layout: post
title:  " The maximum column size is 767 bytes."
date:   2018-4-1 17:10:01 +0800
categories: jpa
---

#### 背景
使用springboot+jpa+mysql初始化表时报如下异常：
```cmd
Hibernate: alter table role add constraint UK_8sewwnpamngi6b1dwaa88askk unique (name)
2019-01-24 16:50:26.572  WARN 2462 --- [ost-startStop-1] o.h.t.s.i.ExceptionHandlerLoggedImpl     : GenerationTarget encountered exception accepting command : Error executing DDL via JDBC Statement

org.hibernate.tool.schema.spi.CommandAcceptanceException: Error executing DDL via JDBC Statement
	at org.hibernate.tool.schema.internal.exec.GenerationTargetToDatabase.accept(GenerationTargetToDatabase.java:67) ~[hibernate-core-5.2.17.Final.jar:5.2.17.Final]
	at org.hibernate.tool.schema.internal.SchemaCreatorImpl.applySqlString(SchemaCreatorImpl.java:440) [hibernate-core-5.2.17.Final.jar:5.2.17.Final]
	at org.hibernate.tool.schema.internal.SchemaCreatorImpl.applySqlStrings(SchemaCreatorImpl.java:424) [hibernate-core-5.2.17.Final.jar:5.2.17.Final]
	at org.hibernate.tool.schema.internal.SchemaCreatorImpl.createFromMetadata(SchemaCreatorImpl.java:349) [hibernate-core-5.2.17.Final.jar:5.2.17.Final]
	at org.hibernate.tool.schema.internal.SchemaCreatorImpl.performCreation(SchemaCreatorImpl.java:166) [hibernate-core-5.2.17.Final.jar:5.2.17.Final]
	at org.hibernate.tool.schema.internal.SchemaCreatorImpl.doCreation(SchemaCreatorImpl.java:135) [hibernate-core-5.2.17.Final.jar:5.2.17.Final]
	at org.hibernate.tool.schema.internal.SchemaCreatorImpl.doCreation(SchemaCreatorImpl.java:121) [hibernate-core-5.2.17.Final.jar:5.2.17.Final]
	at org.hibernate.tool.schema.spi.SchemaManagementToolCoordinator.performDatabaseAction(SchemaManagementToolCoordinator.java:155) [hibernate-core-5.2.17.Final.jar:5.2.17.Final]
	at org.hibernate.tool.schema.spi.SchemaManagementToolCoordinator.process(SchemaManagementToolCoordinator.java:72) [hibernate-core-5.2.17.Final.jar:5.2.17.Final]
	at org.hibernate.internal.SessionFactoryImpl.<init>(SessionFactoryImpl.java:312) [hibernate-core-5.2.17.Final.jar:5.2.17.Final]
	at org.hibernate.boot.internal.SessionFactoryBuilderImpl.build(SessionFactoryBuilderImpl.java:462) [hibernate-core-5.2.17.Final.jar:5.2.17.Final]
	at org.hibernate.jpa.boot.internal.EntityManagerFactoryBuilderImpl.build(EntityManagerFactoryBuilderImpl.java:892) [hibernate-core-5.2.17.Final.jar:5.2.17.Final]
	at org.springframework.orm.jpa.vendor.SpringHibernateJpaPersistenceProvider.createContainerEntityManagerFactory(SpringHibernateJpaPersistenceProvider.java:57) [spring-orm-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean.createNativeEntityManagerFactory(LocalContainerEntityManagerFactoryBean.java:365) [spring-orm-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.buildNativeEntityManagerFactory(AbstractEntityManagerFactoryBean.java:390) [spring-orm-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.afterPropertiesSet(AbstractEntityManagerFactoryBean.java:377) [spring-orm-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean.afterPropertiesSet(LocalContainerEntityManagerFactoryBean.java:341) [spring-orm-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.invokeInitMethods(AbstractAutowireCapableBeanFactory.java:1767) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1704) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:581) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:503) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:317) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:222) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:315) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:199) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveReference(BeanDefinitionValueResolver.java:367) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveValueIfNecessary(BeanDefinitionValueResolver.java:110) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.ConstructorResolver.resolveConstructorArguments(ConstructorResolver.java:625) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:444) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateUsingFactoryMethod(AbstractAutowireCapableBeanFactory.java:1256) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1105) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:543) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:503) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveInnerBean(BeanDefinitionValueResolver.java:312) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.BeanDefinitionValueResolver.resolveValueIfNecessary(BeanDefinitionValueResolver.java:131) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.applyPropertyValues(AbstractAutowireCapableBeanFactory.java:1611) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1363) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:580) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:503) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:317) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:222) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:315) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:199) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.config.DependencyDescriptor.resolveCandidate(DependencyDescriptor.java:251) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1138) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1065) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:584) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.annotation.InjectionMetadata.inject(InjectionMetadata.java:91) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor.postProcessPropertyValues(AutowiredAnnotationBeanPostProcessor.java:373) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1350) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:580) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:503) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:317) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:222) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:315) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:199) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.config.DependencyDescriptor.resolveCandidate(DependencyDescriptor.java:251) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1138) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1065) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:584) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.annotation.InjectionMetadata.inject(InjectionMetadata.java:91) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor.postProcessPropertyValues(AutowiredAnnotationBeanPostProcessor.java:373) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1350) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:580) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:503) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:317) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:222) ~[spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:315) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:204) [spring-beans-5.0.7.RELEASE.jar:5.0.7.RELEASE]
	at org.springframework.boot.web.servlet.ServletContextInitializerBeans.getOrderedBeansOfType(ServletContextInitializerBeans.java:226) ~[spring-boot-2.0.3.RELEASE.jar:2.0.3.RELEASE]
	at org.springframework.boot.web.servlet.ServletContextInitializerBeans.addAsRegistrationBean(ServletContextInitializerBeans.java:182) ~[spring-boot-2.0.3.RELEASE.jar:2.0.3.RELEASE]
	at org.springframework.boot.web.servlet.ServletContextInitializerBeans.addAsRegistrationBean(ServletContextInitializerBeans.java:177) ~[spring-boot-2.0.3.RELEASE.jar:2.0.3.RELEASE]
	at org.springframework.boot.web.servlet.ServletContextInitializerBeans.addAdaptableBeans(ServletContextInitializerBeans.java:159) ~[spring-boot-2.0.3.RELEASE.jar:2.0.3.RELEASE]
	at org.springframework.boot.web.servlet.ServletContextInitializerBeans.<init>(ServletContextInitializerBeans.java:81) ~[spring-boot-2.0.3.RELEASE.jar:2.0.3.RELEASE]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.getServletContextInitializerBeans(ServletWebServerApplicationContext.java:250) ~[spring-boot-2.0.3.RELEASE.jar:2.0.3.RELEASE]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.selfInitialize(ServletWebServerApplicationContext.java:237) ~[spring-boot-2.0.3.RELEASE.jar:2.0.3.RELEASE]
	at org.springframework.boot.web.embedded.tomcat.TomcatStarter.onStartup(TomcatStarter.java:54) ~[spring-boot-2.0.3.RELEASE.jar:2.0.3.RELEASE]
	at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5245) ~[tomcat-embed-core-8.5.31.jar:8.5.31]
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150) ~[tomcat-embed-core-8.5.31.jar:8.5.31]
	at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1421) ~[tomcat-embed-core-8.5.31.jar:8.5.31]
	at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1411) ~[tomcat-embed-core-8.5.31.jar:8.5.31]
	at java.util.concurrent.FutureTask.run$$$capture(FutureTask.java:266) ~[na:1.8.0_202]
	at java.util.concurrent.FutureTask.run(FutureTask.java) ~[na:1.8.0_202]
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149) ~[na:1.8.0_202]
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624) ~[na:1.8.0_202]
	at java.lang.Thread.run(Thread.java:748) ~[na:1.8.0_202]
Caused by: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Specified key was too long; max key length is 1000 bytes
	at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method) ~[na:1.8.0_202]
	at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62) ~[na:1.8.0_202]
	at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45) ~[na:1.8.0_202]
	at java.lang.reflect.Constructor.newInstance(Constructor.java:423) ~[na:1.8.0_202]
	at com.mysql.jdbc.Util.handleNewInstance(Util.java:425) ~[mysql-connector-java-5.1.46.jar:5.1.46]
	at com.mysql.jdbc.Util.getInstance(Util.java:408) ~[mysql-connector-java-5.1.46.jar:5.1.46]
	at com.mysql.jdbc.SQLError.createSQLException(SQLError.java:944) ~[mysql-connector-java-5.1.46.jar:5.1.46]
	at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3976) ~[mysql-connector-java-5.1.46.jar:5.1.46]
	at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3912) ~[mysql-connector-java-5.1.46.jar:5.1.46]
	at com.mysql.jdbc.MysqlIO.sendCommand(MysqlIO.java:2530) ~[mysql-connector-java-5.1.46.jar:5.1.46]
	at com.mysql.jdbc.MysqlIO.sqlQueryDirect(MysqlIO.java:2683) ~[mysql-connector-java-5.1.46.jar:5.1.46]
	at com.mysql.jdbc.ConnectionImpl.execSQL(ConnectionImpl.java:2482) ~[mysql-connector-java-5.1.46.jar:5.1.46]
	at com.mysql.jdbc.ConnectionImpl.execSQL(ConnectionImpl.java:2440) ~[mysql-connector-java-5.1.46.jar:5.1.46]
	at com.mysql.jdbc.StatementImpl.executeInternal(StatementImpl.java:845) ~[mysql-connector-java-5.1.46.jar:5.1.46]
	at com.mysql.jdbc.StatementImpl.execute(StatementImpl.java:745) ~[mysql-connector-java-5.1.46.jar:5.1.46]
	at com.zaxxer.hikari.pool.ProxyStatement.execute(ProxyStatement.java:95) ~[HikariCP-2.7.9.jar:na]
	at com.zaxxer.hikari.pool.HikariProxyStatement.execute(HikariProxyStatement.java) ~[HikariCP-2.7.9.jar:na]
	at org.hibernate.tool.schema.internal.exec.GenerationTargetToDatabase.accept(GenerationTargetToDatabase.java:54) ~[hibernate-core-5.2.17.Final.jar:5.2.17.Final]
	... 85 common frames omitted

```
实体类如下：

```java
@Entity
@Table(name = "role")
@Getter
@Setter
@ToString
public class Role {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    @Column(unique = true)
    private String name;
}

```

#### 原因分析猜测：

MySQL5.6版本对需要做索引的字段做了限制，长度不能超过767 bytes，即length小于225。

#### 验证猜测
1. 修改代码如下，去掉unique=true,不指定长度，运行没有问题
    ```java
    @Entity
    @Table(name = "role")
    @Getter
    @Setter
    @ToString
    public class Role {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        @Column
        private String name;
    }
    ```

2. 修改代码如下，增加unique=true，同时指定长度 length=1000,报错。

   ```java
    @Entity
    @Table(name = "role")
    @Getter
    @Setter
    @ToString
    public class Role {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        @Column(unique = true,length=1000)
        private String name;
    }
   ```
3. 修改代码如下，增加unique=true，同时指定长度 length=225,正常。

验证无误，但是至于官网文档中具体是哪句话指定这种限制，并没有找到。

#### 另：Mysql在V5.1之前默认引擎为MyISAM，之后默认存储引擎是InnoDB，如果需要修改，可以指定如下：
```properties
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
spring.jpa.database-platform=org.hibernate.dialect.MySQL5InnoDBDialect
```
