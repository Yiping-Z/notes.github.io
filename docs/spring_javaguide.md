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

# 将一个类声明为 Bean 的注解有哪些?<br>
@Component：通用的注解，可标注任意类为 Spring 组件。如果一个 Bean 不知道属于哪个层，可以使用@Component 注解标注。<br>
@Repository : 对应持久层即 Dao 层，主要用于数据库相关操作。<br>
@Service : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao 层。<br>
@Controller : 对应 Spring MVC 控制层，主要用于接受用户请求并调用 Service 层返回数据给前端页面。

@Component 注解作用于类，而@Bean注解作用于方法。
Spring 内置的 @Autowired 以及 JDK 内置的 @Resource 和 @Inject 都可以用于注入 Bean。

@Autowired 是 Spring 提供的注解，@Resource 是 JDK 提供的注解。Autowired 默认的注入方式为byType（根据类型进行匹配），@Resource默认注入方式为 byName（根据名称进行匹配）。当一个接口存在多个实现类的情况下，@Autowired 和@Resource都需要通过名称才能正确匹配到对应的 Bean。Autowired 可以通过 @Qualifier 注解来显式指定名称，@Resource可以通过 name 属性来显式指定名称。

- singleton : IoC 容器中只有唯一的 bean 实例。Spring 中的 bean 默认都是单例的，是对单例设计模式的应用。
- prototype : 每次获取都会创建一个新的 bean 实例。也就是说，连续 getBean() 两次，得到的是不同的 Bean 实例。
- request （仅 Web 应用可用）: 每一次 HTTP 请求都会产生一个新的 bean（请求 bean），该 bean 仅在当前 HTTP request 内有效。
- session （仅 Web 应用可用） : 每一次来自新 session 的 HTTP 请求都会产生一个新的 bean（会话 bean），该 bean 仅在当前 HTTP session 内有效
- application/global-session （仅 Web 应用可用）：每个 Web 应用在启动时创建一个 Bean（应用 Bean），该 bean 仅在当前应用启动时间内有效。
- websocket （仅 Web 应用可用）：每一次 WebSocket 会话产生一个新的 bean。

# Bean 的生命周期了解么?<br>
Bean 容器找到配置文件中 Spring Bean 的定义。<br>
Bean 容器利用 Java Reflection API 创建一个 Bean 的实例。<br>
如果涉及到一些属性值 利用 set()方法设置一些属性值。<br>
如果 Bean 实现了 BeanNameAware 接口，调用 setBeanName()方法，传入 Bean 的名字。<br>
如果 Bean 实现了 BeanClassLoaderAware 接口，调用 setBeanClassLoader()方法，传入 ClassLoader对象的实例。<br>
如果 Bean 实现了 BeanFactoryAware 接口，调用 setBeanFactory()方法，传入 BeanFactory对象的实例。<br>
与上面的类似，如果实现了其他 *.Aware接口，就调用相应的方法。<br>
如果有和加载这个 Bean 的 Spring 容器相关的 BeanPostProcessor 对象，执行postProcessBeforeInitialization() 方法<br>
如果 Bean 实现了InitializingBean接口，执行afterPropertiesSet()方法。
如果 Bean 在配置文件中的定义包含 init-method 属性，执行指定的方法。<br>
如果有和加载这个 Bean 的 Spring 容器相关的 BeanPostProcessor 对象，执行postProcessAfterInitialization() 方法<br>
当要销毁 Bean 的时候，如果 Bean 实现了 DisposableBean 接口，执行 destroy() 方法。<br>
当要销毁 Bean 的时候，如果 Bean 在配置文件中的定义包含 destroy-method 属性，执行指定的方法

# AOP(Aspect-Oriented Programming:面向切面编程)
能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性。

Spring AOP 就是基于动态代理的，如果要代理的对象，实现了某个接口，那么 Spring AOP 会使用 JDK Proxy，去创建代理对象，而对于没有实现接口的对象，就无法使用 JDK Proxy 去进行代理了，这时候 Spring AOP 会使用 Cglib 生成一个被代理对象的子类来作为代理

| 术语     | 含义 |
| ----------- | ----------- |
| 目标(Target)  | 被通知的对象      |
| 代理(Proxy)   | 向目标对象应用通知之后创建的代理对象    |
| 连接点(JoinPoint)	| 目标对象的所属类中，定义的所有方法均为连接点| 
| 切入点(Pointcut)	| 被切面拦截 / 增强的连接点（切入点一定是连接点，连接点不一定是切入点）| 
| 通知(Advice)	| 增强的逻辑 / 代码，也即拦截到目标对象的连接点之后要做的事情| 
| 切面(Aspect)	| 切入点(Pointcut)+通知(Advice)| 
| Weaving(织入)| 	将通知应用到目标对象，进而生成代理对象的过程动作| 

Spring AOP 属于运行时增强，而 AspectJ 是编译时增强。
Spring AOP 基于代理(Proxying)，而 AspectJ 基于字节码操作(Bytecode Manipulation)。

Model: Java Bean
View: JSP (Jakarta Server Pages)
Controller: Servlet

# MVC2
Spring MVC 下我们一般把后端项目分为 Service 层（处理业务）、Dao 层（数据库操作）、Entity 层（实体类）、Controller 层(控制层，返回数据给前台页面)。

# Spring MVC
- DispatcherServlet：核心的中央处理器，负责接收请求、分发，并给予客户端响应。
- HandlerMapping：处理器映射器，根据 uri 去匹配查找能处理的 Handler ，并会将请求涉及到的拦截器和 Handler 一起封装。
- HandlerAdapter：处理器适配器，根据 HandlerMapping 找到的 Handler ，适配执行对应的 Handler；
- Handler：请求处理器，处理实际请求的处理器。
- ViewResolver：视图解析器，根据 Handler 返回的逻辑视图 / 视图，解析并渲染真正的视图，并传递给 DispatcherServlet 响应客户端

# 流程
- 客户端（浏览器）发送请求， DispatcherServlet拦截请求。
- DispatcherServlet 根据请求信息调用 HandlerMapping 。
- HandlerMapping 根据 uri 去匹配查找能处理的 Handler（也就是我们平常说的 Controller 控制器） ，并会将请求涉及到的拦截器和 Handler 一起封装。
- DispatcherServlet 调用 HandlerAdapter适配器执行 Handler 。
- Handler 完成对用户请求的处理后，会返回一个 ModelAndView 对象给DispatcherServlet，ModelAndView 顾名思义，包含了数据模型以及相应的视图的信息。Model 是返回的数据对象，View 是个逻辑上的 View。
- ViewResolver 会根据逻辑 View 查找实际的 View。
DispaterServlet 把返回的 Model 传给 View（视图渲染）。
把 View 返回给请求者（浏览器)

# 统一异常处理
具体会使用到@ControllerAdvice + @ExceptionHandler 这两个注解。

# Spring 框架中用到了哪些设计模式？
- 工厂设计模式 : Spring 使用工厂模式通过 BeanFactory、ApplicationContext 创建 bean 对象。
- 代理设计模式 : Spring AOP 功能的实现。
- 单例设计模式 : Spring 中的 Bean 默认都是单例的。
- 模板方法模式 : Spring 中 jdbcTemplate、hibernateTemplate 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。
- 包装器设计模式 : 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。
- 观察者模式: Spring 事件驱动模型就是观察者模式很经典的一个应用。
- 适配器模式 : Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配Controller。

# Spring 管理事务的方式有几种？
- 编程式事务：在代码中硬编码(不推荐使用) : 通过 TransactionTemplate或者 TransactionManager 手动管理事务，实际应用中很少使用，但是对于你理解 Spring 事务管理原理有帮助。
- 声明式事务：在 XML 配置文件中配置或者直接基于注解（推荐使用） : 实际是通过 AOP 实现（基于@Transactional 的全注解方式使用最多）

# Spring 事务隔离级别
- TransactionDefinition.ISOLATION_DEFAULT :使用后端数据库默认的隔离级别，MySQL 默认采用的 REPEATABLE_READ 隔离级别 Oracle 默认采用的 READ_COMMITTED 隔离级别.
- TransactionDefinition.ISOLATION_READ_UNCOMMITTED :最低的隔离级别，使用这个隔离级别很少，因为它允许读取尚未提交的数据变更，可能会导致脏读、幻读或不可重复读
- TransactionDefinition.ISOLATION_READ_COMMITTED : 允许读取并发事务已经提交的数据，可以阻止脏读，但是幻读或不可重复读仍有可能发生
- TransactionDefinition.ISOLATION_REPEATABLE_READ : 对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，可以阻止脏读和不可重复读，但幻读仍有可能发生。
- TransactionDefinition.ISOLATION_SERIALIZABLE : 最高的隔离级别，完全服从 ACID 的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，该级别可以防止脏读、不可重复读以及幻读。但是这将严重影响程序的性能。通常情况下也不会用到该级别。

# @Transactional(rollbackFor = Exception.class)注解了解吗？
rollbackFor=Exception.class,可以让事务在遇到非运行时异常时也回滚。

JPA: Java Persistence API

