#SpringBoot 相关


## 汪超  2019-01-25

### 文档编写记录

版本    |   说明    |   日期   | 编写者 
-------| ----------| ---------| --------
 0.1   | 记录 |  2019-01-25 |  汪超
 
 
## 启动Servlet 

在SpringBoot的配置文件中添加以下配置即可：

```
spring:
	mvc: 
		servlet:
			load-on-startup: 1
```

```
spring boot 默认配置三个深坑，一个比一个耗时
1、dispatcherServlet 是懒加载的
2、数据库链接是懒加载的
3、linux 下真随机数生成器


\1. 设置 spring.mvc.servlet.load-on-startup=1
\2. 启动方法拿个 dao 的 bean，跑个小查询
\3. 加启动参数 -Djava.security.egd=file:/dev/./urandom

```

# Undertow 容器

https://www.jianshu.com/p/9710585258fb

``` xml
<dependencies>
    <!--Spring boot starter-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-undertow</artifactId>
    </dependency>
    <!--/Spring boot starter-->
</dependencies>
```

``` java
// 在@Configuration的类中添加@bean
 @Bean
 UndertowEmbeddedServletContainerFactory embeddedServletContainerFactory() {
    
    UndertowEmbeddedServletContainerFactory factory = new UndertowEmbeddedServletContainerFactory();
    
    // 这里也可以做其他配置
    factory.addBuilderCustomizers(builder -> builder.setServerOption(UndertowOptions.ENABLE_HTTP2, true));
    
    return factory;
 }
```