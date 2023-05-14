---
layout: notes
title: Spring5 AOP, MVC, IOC (pdai)
description: 
image: 
---
**Inversion of Control** Container 
Dependency Injection
Java without Enterprise JavaBeans (EJBs)
Allow enterprise development without application server
Plain Old Java Objects (POJO)
Unobtrusive
AOP/Proxies
Best Practices
Testability/Maintainability/Scalability/Complexity/Business Focus
WORA: Write Once Run Anywhere
AppConfig @Configuration
@Bean
Setter ingestion/constructor ingestion
Spring Scopes and Autowiring
Scopes:
Singleton: One instantiation, single instance per Spring container @Scope("singleton")
Prototype: new bean per request
@Scope("prototype")
Valid only in web-aware Spring projects: Request, Session, Global
![在这里插入图片描述](https://img-blog.csdnimg.cn/b39376266832483c8da261fe370604e2.png)

**AOP**
连接点(Jointpoint):在哪里干;
切入点(Pointcut): 在哪里于的集合;
通知(Advice)为于什么;
方面/切面(Aspect):于什么(引入什么);
目标对象(Target Object):在AOP中表示为对谁于;
织入(Weaving):怎么实现的;
AOP代理(AOP Proxy):怎么实现的一种典型方式;
前置通知(Before advice):在某连接点之前执行的通知,但这个通知不能阻止连接点之前的执行流程(除非它 抛出一个异常)。
后置通知(After returning advice):在某连接点正常完成后执行的通知:例如,一个方法没有抛出任何异常, 正常返回。
异常通知(After throwing advice):在方法抛出异常退出时执行的通知。
最终通知(After (finally) advice):当某连接点退出的时候执行的通知(不论是正常返回还是异常退出)。 
环绕通知(Around Advice):包围一个连接点的通知,如方法调用。这是最强大的一种通知类型。环绕通知可 以在方法调用前后完成自定义的行为。它也会选择是否继续执行连接点或直接返回它自己的返回值或抛出异常来 结束执行。
![在这里插入图片描述](https://img-blog.csdnimg.cn/ddbf5153bcf14caea102a043cfba3532.png)
动态织入的方式是在运行时动态将要增强的代码织入到目标类中,这样往往是通过动态代理技术完成的,如Java JDK的动态代理(Proxy. 底层通过反射实现)或者CGLIB的动态代理(底层通过继承实现), Spring AOP采用的就是基于 运行时增强的代理技术Apectu采用的就是静态织入的方式。Apectu主要采用的是编译期织入,在这个期间使用
Aspect的acj编译器(类似javac)把aspect类编译成class字节码后,在java目标类编译时织入,即先编译aspect类再编 译目标类。![在这里插入图片描述](https://img-blog.csdnimg.cn/9e67f38dfb2d4745a56d521acb40ae2f.png#pic_center)
**Model, View , Controller**![在这里插入图片描述](https://img-blog.csdnimg.cn/ef516aef2ab54a799c5a8af7bd5037c4.png)
**BeanFactory**： 工厂模式定义了IOC容器的基本功能规范
**ListableBeanFactory**、**HierarchicalBeanFactory** 和**AutowireCapableBeanFactory**
**BeanRegistry**： 向IOC容器手工注册 BeanDefinition 对象的方法
**BeanDefinition** 定义了各种Bean对象及其相互的关系**BeanDefinitionReader** 这是BeanDefinition的解析器BeanDefinitionHolder 这是BeanDefination的包装类，用来存储BeanDefinition，name以及aliases等。

ApplicationContext：IOC接口设计和实现
访问资源： 对不同方式的Bean配置（即资源）进行加载。(实现ResourcePatternResolver接口)
国际化: 支持信息源，可以实现国际化。（实现MessageSource接口）
应用事件: 支持应用事件。(实现ApplicationEventPublisher接口)

**GenericApplicationContext**： 是初始化的时候就创建容器，往后的每次refresh都不会更改
**AbstractRefreshableApplicationContext**： AbstractRefreshableApplicationContext及子类的每次refresh都是先清除已有(如果不存在就创建)的容器，然后再重新创建；AbstractRefreshableApplicationContext及子类无法做到GenericApplicationContext混合搭配从不同源头获取bean的定义信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/75fe79faaffc4fca9e61ae3d0b40dc11.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/51446c89542840cebefd9a5d923349e8.png)
obtainFreshBeanFactory
loadBeanDefinitions
AbstractBeanDefinitionReader读取Bean定义资源
XmlBeanDefinitionReader加载Bean定义资源
DocumentLoader将Bean定义资源转换为Document对象
XmlBeanDefinitionReader解析载入的Bean定义资源文件
DefaultBeanDefinitionDocumentReader对Bean定义的Document对象解析
BeanDefinitionParserDelegate解析Bean定义资源文件生成BeanDefinition
解析过后的BeanDefinition在IoC容器中的注册
DefaultListableBeanFactory向IoC容器注册解析后的BeanDefinition
![在这里插入图片描述](https://img-blog.csdnimg.cn/a2fd6e48823d4a14a92739fda99d2f48.png)
解析bean的真正name，如果bean是工厂类，name前缀会加&，需要去掉无参单例先从缓存中尝试获取如果bean实例还在创建中，则直接抛出异常如果bean definition 存在于父的bean工厂中，委派给父Bean工厂获取标记这个beanName的实例正在创建确保它的依赖也被初始化真正创建 单例时原型时根据bean的scope创建
第一层缓存（singletonObjects）：单例对象缓存池，已经实例化并且属性赋值，这里的对象是成熟对象；
第二层缓存（earlySingletonObjects）：单例对象缓存池，已经实例化但尚未属性赋值，这里的对象是半成品对象；
第三层缓存（singletonFactories）: 单例工厂的缓存

“A对象setter依赖B对象，B对象setter依赖A对象”，A首先完成了初始化的第一步，而且将本身提早曝光到singletonFactories中，此时进行初始化的第二步，发现本身依赖对象B，此时就尝试去get(B)，发现B尚未被create，因此走create流程，B在初始化第一步的时候发现本身依赖了对象A，因而尝试get(A)，尝试一级缓存singletonObjects(确定没有，由于A还没初始化彻底)，尝试二级缓存earlySingletonObjects（也没有），尝试三级缓存singletonFactories，因为A经过ObjectFactory将本身提早曝光了，因此B可以经过ObjectFactory.getObject拿到A对象(半成品)，B拿到A对象后顺利完成了初始化阶段一、二、三，彻底初始化以后将本身放入到一级缓存singletonObjects中。此时返回A中，A此时能拿到B的对象顺利完成本身的初始化阶段二、三，最终A也完成了初始化，进去了一级缓存singletonObjects中，并且更加幸运的是，因为B拿到了A的对象引用，因此B如今hold住的A对象完成了初始化。

Spring无法解决除单例模式以外的循环依赖（构造器/prototype/多例）
解决方法：
- 使用@Lazy注解，延迟加载
- 使用@DependsOn注解，指定加载先后关系
- 修改文件名称，改变循环依赖类的加载顺序
![在这里插入图片描述](https://img-blog.csdnimg.cn/56a7df1d613a43b3a3fc1e098057c8cc.png)
如果 BeanFactoryPostProcessor 和 Bean 关联, 则调用**postProcessBeanFactory**方法.(即首先尝试从Bean工厂中获取Bean)
如果 InstantiationAwareBeanPostProcessor 和 Bean 关联，则调用**postProcessBeforeInstantiation**方法
根据配置情况调用 Bean 构造方法**实例化 Bean**。
利用**依赖注入**完成 Bean 中所有属性值的配置注入。
如果 InstantiationAwareBeanPostProcessor 和 Bean 关联，则调用**postProcessAfterInstantiation**方法和postProcessProperties
调用xxxAware接口 (BeanNameAware/BeanClassLoaderAware/BeanFactoryAware/EnvironmentAware/EmbeddedValueResolverAware/ApplicationContextAware)
如果 Bean 实现了 InitializingBean 接口，则 Spring 将调用 **afterPropertiesSet()** 方法。(或者有执行@PostConstruct注解的方法)
如果在配置文件中通过**init-method**属性指定了初始化方法，则调用该初始化方法。
如果 BeanPostProcessor 和 Bean 关联，则 Spring 将调用该接口的初始化方法 **postProcessAfterInitialization()**。此时，Bean 已经可以被应用系统使用了。
如果在 <bean> 中指定了该 Bean 的作用范围为 scope="singleton"，则将该 Bean 放入 Spring IoC 的缓存池中，将触发 Spring 对该 Bean 的生命周期管理；
如果在 <bean> 中指定了该 Bean 的作用范围为 scope="prototype"，则将该 Bean 交给调用者，调用者管理该 Bean 的生命周期，Spring 不再管理该 Bean。
如果 Bean 实现了 DisposableBean 接口，则 Spring 会调用 **destory()** 方法将 Spring 中的 Bean 销毁；(或者有执行@PreDestroy注解的方法)
如果在配置文件中通过 destory-method 属性指定了 Bean 的销毁方法，则 Spring 将调用该方法对 Bean 进行销毁。

AOP的创建工作是交给AnnotationAwareAspectJAutoProxyCreator来完成
实现了两类接口：BeanFactoryAware属于Bean级生命周期接口方法InstantiationAwareBeanPostProcessor 和 BeanPostProcessor 这两个接口实现，一般称它们的实现类为“后处理器”，是容器级生命周期接口方法；
核心的初始化方法肯定在postProcessBeforeInstantiation和postProcessAfterInitialization中

处理使用了@Aspect注解的切面类，然后将切面类的所有切面方法根据使用的注解生成对应Advice，并将Advice连同切入点匹配器和切面类等信息一并封装到Advisor的过程。

 - 由IOC Bean加载方法栈中找到parseCustomElement方法，找到parse aop:aspectj-autoproxy的handler(org.springframework.aop.config.AopNamespaceHandler)
 - AopNamespaceHandler注册了<aop:aspectj-autoproxy/>的解析类是AspectJAutoProxyBeanDefinitionParser
 - AspectJAutoProxyBeanDefinitionParser的parse 方法 通过AspectJAwareAdvisorAutoProxyCreator类去创建
 - AspectJAwareAdvisorAutoProxyCreator实现了两类接口，BeanFactoryAware和BeanPostProcessor；根据Bean生命周期方法找到两个核心方法：
postProcessBeforeInstantiation和postProcessAfterInitialization 
 	- postProcessBeforeInstantiation：主要是处理使用了@Aspect注解的切面类，然后将切面类的所有切面方法根据使用的注解生成对应Advice，并将Advice连同切入点匹配器和切面类等信息一并封装到Advisor
	 - postProcessAfterInitialization：主要负责将Advisor注入到合适的位置，创建代理（cglib或jdk)，为后面给代理进行增强实现做准备。

Spring默认在目标类实现接口时是通过JDK代理实现的，只有非接口的是通过Cglib代理实现的。当设置proxy-target-class为true时在目标类不是接口或者代理类时优先使用cglib代理实现。

代理模式(Proxy pattern): 为另一个对象提供一个替身或占位符以控制对这个对象的访问
Cglib是一个强大的、高性能的代码生成包，它广泛被许多AOP框架使用，为他们提供方法的拦截。
![在这里插入图片描述](https://img-blog.csdnimg.cn/9dd0ce506f1740958cdd6eb35695d564.png)
**JDK代理**：
第一步：准备工作，将所有方法包装成ProxyMethod对象，包括Object类中hashCode、equals、toString方法，以及被代理的接口中的方法
第二步：为代理类组装字段，构造函数，方法，static初始化块等
第三步：写入class文件

MVC:
![在这里插入图片描述](https://img-blog.csdnimg.cn/67c56cc30363437d83b5658e5c501ec4.png)
HandlerMapping是映射处理器
HandlerAdpter是处理适配器，用来找到你的Controller中的处理方法
HandlerExceptionResolver是当遇到处理异常时的异常解析器

**首先用户发送请求——>DispatcherServlet**，前端控制器收到请求后自己不进行处理，而是委托给其他的解析器进行 处理，作为统一访问点，进行全局的流程控制；
**DispatcherServlet——>HandlerMapping**， HandlerMapping 将会把请求映射为 HandlerExecutionChain 对象（包含一 个Handler 处理器（页面控制器）对象、多个HandlerInterceptor 拦截器）对象，通过这种策略模式，很容易添加新 的映射策略；**DispatcherServlet——>HandlerAdapter**，HandlerAdapter 将会把处理器包装为适配器，从而支持多种类型的处理器， 即适配器设计模式的应用，从而很容易支持很多类型的处理器；**HandlerAdapter——>处理器功能处理方法的调用**，HandlerAdapter 将会根据适配的结果调用真正的处理器的功能处 理方法，完成功能处理；并返回一个ModelAndView 对象（包含模型数据、逻辑视图名）；
**ModelAndView 的逻辑视图名——> ViewResolver**，ViewResolver 将把逻辑视图名解析为具体的View，通过这种策 略模式，很容易更换其他视图技术；
**View——>渲染**，View 会根据传进来的Model 模型数据进行渲染，此处的Model 实际是一个Map 数据结构，因此 很容易支持其他视图技术；
**返回控制权给DispatcherServlet**，由DispatcherServlet 返回响应给用户，到此一个流程结束。