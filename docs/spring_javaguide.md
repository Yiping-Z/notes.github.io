---
layout: 
title: Spring (Java Guide)
description: 
image: 
hide:
    -toc
---
# Core Container
Spring 框架的核心模块，也可以说是基础模块，主要提供 IoC 依赖注入功能的支持。Spring 其他所有的功能基本都需要依赖于该模块，我们从上面那张 Spring 各个模块的依赖关系图就可以看出来。<br>
spring-core：Spring 框架基本的核心工具类。<br>
spring-beans：提供对 bean 的创建、配置和管理等功能的支持。<br>
spring-context：提供对国际化、事件传播、资源加载等功能的支持。<br>
spring-expression：提供对表达式语言（Spring Expression Language） SpEL 的支持，只依赖于 core 模块，不依赖于其他模块，可以单独使用。

# AOP
spring-aspects：该模块为与 AspectJ 的集成提供支持。<br>
spring-aop：提供了面向切面的编程实现。<br>
spring-instrument：提供了为 JVM 添加代理（agent）的功能。 具体来讲，它为 Tomcat 提供了一个织入代理，能够为 Tomcat 传递类文 件，就像这些文件是被类加载器加载的一样。没有理解也没关系，这个模块的使用场景非常有限。

# Data Access/Integration
spring-jdbc：提供了对数据库访问的抽象 JDBC。不同的数据库都有自己独立的 API 用于操作数据库，而 Java 程序只需要和 JDBC API 交互，这样就屏蔽了数据库的影响。<br>
spring-tx：提供对事务的支持。<br>
spring-orm：提供对 Hibernate、JPA、iBatis 等 ORM 框架的支持。<br>
spring-oxm：提供一个抽象层支撑 OXM(Object-to-XML-Mapping)，例如：JAXB、Castor、XMLBeans、JiBX 和 XStream 等。<br>
spring-jms : 消息服务。自 Spring Framework 4.1 以后，它还提供了对 spring-messaging 模块的继承。

# Spring Web
spring-web：对 Web 功能的实现提供一些最基础的支持。<br>
spring-webmvc：提供对 Spring MVC 的实现。<br>
spring-websocket：提供了对 WebSocket 的支持，WebSocket 可以让客户端和服务端进行双向通信。<br>
spring-webflux：提供对 WebFlux 的支持。WebFlux 是 Spring Framework 5.0 中引入的新的响应式框架。与 Spring MVC 不同，它不需要 Servlet API，是完全异步。

MVC: 模型(Model)、视图(View)、控制器(Controller)

Spring 旨在简化 J2EE 企业应用程序开发。Spring Boot 旨在简化 Spring 开发（减少配置文件，开箱即用！）。

将一个类声明为 Bean 的注解有哪些?<br>
@Component：通用的注解，可标注任意类为 Spring 组件。如果一个 Bean 不知道属于哪个层，可以使用@Component 注解标注。<br>
@Repository : 对应持久层即 Dao 层，主要用于数据库相关操作。<br>
@Service : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao 层。<br>
@Controller : 对应 Spring MVC 控制层，主要用于接受用户请求并调用 Service 层返回数据给前端页面。



