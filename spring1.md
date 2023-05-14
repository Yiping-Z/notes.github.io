---
layout: notes
title: Spring5 框架, 要点, IOC (pdai)
description: 
image: 
---
## Spring

 ORM/DAO -> POJO (IOC容器) -> Service -> Controller
   - **非侵入式**：基于Spring开发的应用中的对象可以不依赖于Spring的API 
   - **控制反转**：IOC——Inversion of Control，指的是将对象的创建权交给 Spring 去创建。使用 Spring之前，对象的创建都是由我们自己在代码中new创建。而使用 Spring 之后。对象的创建都是给了 Spring 框架。
   - **依赖注入**：DI——Dependency Injection，是指依赖的对象不需要手动调用 setXX 方法去设置，而是通过配置赋值。
   - **面向切面编程**：Aspect Oriented Programming——AOP容器：Spring是一个容器，因为它包含并且管理应用对象的生命周期组件化：Spring 实现了使用简单的组件配置组合成一个复杂的应用。在 Spring中可以使用XML和Java注解组合这些对象。 
   - **一站式**：在 IOC 和 AOP的基础上可以整合各种企业应用的开源框架和优秀的第三方类库（实际上 Spring 自身也提供了表现层的 SpringMVC 和持久层的
   Spring JDBC）

**Core Container:** beans, core, context (Application Context), SpEL
**Data Access/Integration**: JDBC/ORM (Object Relational Mapping)/OXM (Object/XML)/JMS (服务消息)/Transaction
**Web:** web, servlet, websocket, webflux (完全异步且非阻塞), Portlet
**AOP, Aspects, Instrumentation** (类工具的支持和类加载器的实现), **Messaging, JCL** (日志框架集成的模块)
**Test**: Junit, TestNG

```java
 /**
* aspect for every methods under service package.
*/
@Around("execution(* tech.pdai.springframework.service.*.*(..))")
public Object businessService(ProceedingJoinPoint pjp) throws Throwable {
    // get attribute through annotation
    Method method = ((MethodSignature) pjp.getSignature()).getMethod();
    System.out.println("execute method: " + method.getName());

    // continue to process
    return pjp.proceed();
}
```
**@Configuration**: These classes consist principally of @Bean-annotated methods that define instantiation, configuration, and initialization logic for objects that are managed by the Spring IoC container.

```java
@Configuration
public class AppConfig {

    @Bean
    public TransferService transferService() {
        return new TransferServiceImpl();
    }

}
```
**@Service** annotates classes at the service layer.  它将根据用户请求请求@Repository

**@Repository** annotates classes at the persistence layer, which will act as a database repository

**IOC**: 用户管理Bean转变为框架管理Bean
**DI**: 应用程序代码从Ioc Container中获取依赖的Bean，注入到应用程序中
**IOC Config**：XML, 注解， Java
**依赖注入方式**：构造方法注入（Construct注入），setter注入，基于注解的注入（接口注入）
**@Autowired**（自动注入）：Constructor，byType，byName
**构造器注入**的方式能够保证注入的组件不可变，并且确保需要的依赖不为空
@Target(ElementType.CONSTRUCTOR) #构造函数
@Target(ElementType.METHOD) #方法
@Target(ElementType.PARAMETER) #方法参数
@Target(ElementType.FIELD) #字段、枚举的常量
@Target(ElementType.ANNOTATION_TYPE) #注解

**@Resource**
@Target(ElementType.TYPE) #接口、类、枚举、注解
@Target(ElementType.FIELD) #字段、枚举的常量
@Target(ElementType.METHOD) #方法

**@Inject**
@Target(ElementType.CONSTRUCTOR) #构造函数
@Target(ElementType.METHOD) #方法
@Target(ElementType.FIELD) #字段、枚举的常量

- @Autowired、@Inject用法基本一样，不同的是@Inject没有required属性
- @Autowired、@Inject是默认按照类型匹配的，@Resource是按照名称匹配的
- @Autowired如果需要按照名称匹配需要和@Qualifier一起使用，@Inject和@Named一起使用，@Resource则通过name进行指定
