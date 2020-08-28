## Spring

1. [特点](#a)
2. [常用容器](#b)
3. [控制反转](#c)
4. [常用注解](#d)
5. [AOP](#e)
6. [mybatis整合](#f)





<h3 id="a">特点</h3>

- 轻量级 非侵入式

- 控制反转  (IOC)

- 面向切面 （AOP）
- 容器  (bean)
- 开源免费
- 支持事务处理，对框架整合的支持



<h3 id="b">常用容器</h3>

- Spring Core 核心容器
- Spring Context 上下文
- **Spring AOP**
- Spring DAO
- Spring ORM
- Spring WEB
- **Spring MVC**





<h3 id="c">控制反转（IOC）</h3>

IOC——Inversion of Control，控制反转。控制反转并非一种编程技术，而是一种编程思想，因其理论和技术成熟较晚而未能写进设计模式中。



> 不主动生成对象，而是等待对象注入。将对象的创建、赋值和管理等交给外部对象来实现



实现控制反转的方式有两种，一种是**依赖查找**，一种是**依赖注入**，spring使用依赖注入实现控制反转



java中创建对象的方式：

- 构造方法 new
- 反射
- 序列化
- 克隆
- ioc
- 动态代理





spring使用di实现了ioc功能，spring底层创建对象使用的是反射机制

1. 配置文件方式创建对象

```java
//指定配置文件
String config="beans.xml";
//创建表示容器的对象，ApplicationContext
//ClassPathXmlApplicationContext从类路径加载配置文件
ApplicationContext ac=new ClassPathXmlApplicationContext(config)
//从容器中获取对象
SomeService someService=(SomeService)ac.getBean("someServie");
```

- 在spring容器创建时会创建容器内所有的对象
- spring容器创建对象默认调用无参构造方法



注入方式：

1. set注入（设值注入）：spring调用类的set方法，完成属性的赋值

- 在xml文件中 bean 标签下使用 property 标签给属性赋值
  - spring设值注入会先调用构造方法，再调用set方法
  - set方法不能省略
  - 设值注入查找的是方法名
  - 引用类型的注入有两种方式：byName，byType    byName方式引用类型**属性名和bean中的id相同**，byType使用同源关系（同一个类、父子类、接口实现）引入

2. 构造注入：spring调用类的有参构造器，在构造方法中完成赋值

- 在xml文件中 bean 标签下使用 consrtuctor-arg 标签给属性赋值
  - spring会二次扫描配置文件，无需在意bean的顺序
  - index表示参数在构造器中的顺序



<h3 id="d">常用注解</h3>



1. @Component 

- 创建对象，等同于\<bean>

- 属性：value，对象的名称，相当于bean的id，创建的对象在整个spring容器中就一个

- 用在类上
- 组件扫描器

``` xml
	<!--声明组件扫描器 -->
	<context:component-scan base-package="bao">
	<!--指定包第三种扫描方式 -->
	<!--第一种：多次声明组件扫描器，指定不同的包 -->
    <context:component-scan base-package="bao1">
    <context:component-scan base-package="bao2">
        
   	<!--第二种：使用分隔符 ; 或 ，，指定不同的包 -->
    <context:component-scan base-package="bao1,bao2">
    
    <!--第三种：指定父包 -->
    <context:component-scan base-package="bao1">      
        
        
```

2. @Repository

- 用在dao的实现类上，创建dao对象，dao对象访问数据库

3. @Service

- 用在Service实现类上，创建service对象，service对象做业务处理，可以有事务等功能

4. @Controller

- 用在Controller类上，创建控制器对象，能够接收用户提交的参数，显示请求的处理结果



> @Repository,@Service,@Controller用于项目分层，和@Component功能一致



5. @Value

- 给简单的属性赋值
- 属性：value，string类型，表示简单类型的属性值
- 用在属性和set方法上
- @Value("${}")



6. @Autowired

- 实现引用类型的赋值
- 属性：required，boolean类型，引用类型赋值失败是否报错
- 用在属性和set方法上
- 支持byName和byType方式，默认使用byType注入
- @Qualifier根据名字查找对象，byName方式注入



7. @Resource

- 来自jdk中，spring支持这个注解
- 实现引用了型赋值
- 用在属性和set方法上
- 支持byName和byType方式，默认使用byName方式注入，先使用byName，如果赋值失败再使用byType
- 如果想只使用byName，加一个name属性



<h3 id="e">AOP</h3>

AOP（aspect oriented programming），面向切面编程。

AOP底层是采用动态代理模式实现的

AOP是一个规范，是动态代理的规范化和标准，通常使用AspectJ框架实现AOP



1. 动态代理的实现

- jdk动态代理，使用proxy，method和InvocationHandler创建代理对象，必须实现接口
- cglib动态代理，第三方的工具库，创建代理对象，通过继承目标类，创建子类，子类就是代理对象，要求目标类和方法不能使用final关键字修饰

2. 动态代理的作用

- 不改变原有代码的情况下增加业务，专注业务逻辑代码

- 减少重复代码

- 解耦，业务和日志，事务和非事务分离

  

3. AOP可以使用jdk，cglib两种代理方式，是动态代理的规范化

- Aspect：切面，给目标类增加功能，就是切面，切面一般都是非业务方法，可以独立使用。
  - 日志
  - 事务
  - 统计信息
  - 参数检查
  - 权限验证
- JoinPoint：连接点，连接业务方法和切面的位置，某个类中的业务方法
- Pointcut：切入点，多个连接点方法的集合
- 目标对象：给哪个类的方法增加功能
- Advice：通知，通知切面的执行时间



切面的三个要素：

1. 切面的功能代码
2. 切面的执行时间，Advice
3. 切面的位置，Pointcut



4. AspectJ

AspectJ有两种方式实现AOP

- xml配置方式：配置全局事务
- 注解方式
  - 切面的执行时间，Advice（增强）
    - @Before
    - @AfterReturning
    - @Around
    - @AfterThrowing
    - @After
- execution(访问权限 方法返回值 方法声明(参数) 异常)
  - 通配符：* 任意多个字符，·· 多个参数， ++ 当前类及子类、当前接口及实现类
    - execution(public * *(..)) 指定切入点为任意公共方法
    - execution(* set*(..)) 指定切入点为set开始的方法
    - execution(* com.zsf.service.*.*(..)) 指定切入点为service包下的所有类的所有方法



5. AOP实现

   a. 创建目标类：实现接口和方法

   b. 创建切面类：在类的上面加上@Aspect注解，在方法的上面加上通知注解

   c. 创建spring配置文件：声明对象，交给容器统一管理

- 定义方法，方法是实现切面功能的
  - 公共方法public
  
  - 没有返回值
  
  - 自定义名称
  
  - 可以有参也可以无参
    
    - @Before：前置通知，在目标类方法之前执行，不会改变目标方法执行结果，不会影响目标方法执行
    - @AfterReturning：后置通知，在目标方法之后执行，能够获取到目标函数的返回值，并可以修改返回值
      - value：切入点表达式
      - returning：自定义变量，表示目标方法返回值，变量名必须等于形参名
    - @Around：环绕通知，必须有返回值，推荐用Object，方法固定参数ProceedingJoinPoint，在方法前后都能增强功能，修改目标方法的执行结果，影响最后的调用结果，相当于jdk动态代理的InvocationHandler接口
    
    ``` java
    proceedingJoinPoint.proceed(); //目标方法调用
    ```
    
    
    
    - @AfterThrowing：异常通知
    - @After：最终通知，总是会执行，在目标方法后执行
    - @Pointcut：定义和管理切入点
      - 用在方法上，方法名就是切入点表达式的别名
      - 其他的通知中value可以使用别名
    - JoinPoint：要加入切面功能的目标方法
    
    ``` java
    @Before(value="excution(.......)")
    public void before(JoinPoint joinPoint){
        joinPoint.getSignature;//获取方法的定义
        joinPoint.getSignature().getName(); //获取方法名
        joinPoint.getArgs();//按顺序获取方法参数
    }
    ```
  
  
  
  > 有接口时默认是jdk代理方式，没有接口时用的是cglib动态代理，有接口时可以设置cglib代理方式



<h3 id="#f">mybatis整合</h3>

