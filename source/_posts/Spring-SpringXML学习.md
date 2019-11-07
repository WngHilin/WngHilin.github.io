---
title: SpringXML学习
date: 2019-09-17 18:54:25
tags: 
- JavaWeb
- 学习
- 框架
categories: Spring
---

## IOC&DI概述

* **IOC**（Inversion of Control）：其思想是反转资源获取的方向。传统的资源查找方式要求组件向容器发起请求查找资源，作为回应，容器适时地返回资源。而应用了IOC之后，则是容器主动地将资源推送给它所管理的组件，组件所要做的仅是选择一种合适的方式来接受资源。
* **DI**（Dependency Injection) ：—IOC的另一种表述方式：即组件以一些预先定义好的方式接收来自如容器的资源注入。



## 配置Bean

### 通过全类名

* 通过全类名（反射）进行配置（在xml中使用bean标签）

  * class：bean的全类名，通过反射的方式在IOC中其中创建Bean，所以要求Bean中必须有无参数的构造器

  * id：表示容器中的bean，id唯一。通过id找到相应的bean

    ```xml
        <bean id="HelloWorld" class="top.wnghilin.test.HelloWorld">
            <property name="name" value="Spring"></property>
        </bean>
    ```

  * 步骤：

    * 对IOC容器进行实例化
      * BeanFactory ：IOC容器的基本实现
      * **ApplicationContext**：面向使用的Spring框架的开发者，基本都使用该方式
      * ApplicationContext的主要实现类：
        * ClassPathXmlApplicationContext：从类路径下加载配置文件
        * FileSystemXmlApplicationContext：从文件系统中加载配置文件
      * WebApplicationContext是专门为web应用准备的，它允许从相对于WEB根目录的路径中完成初始化工作。
    * 从ApplicationContext中读取Bean的方法
      * 通过id读取：getBean("id值");
      * 用类型获取：getBean(类名.class); //缺点：要求是唯一的Bean



* Spring的依赖注入方法

  * **属性注入**：通过setter方法注入Bean的属性值或以来对象

    * 使用&lt;property>元素的name属性指定Bean的属性名称，value属性或&lt;value>子节点指定属性值

    * 这种方式是最常用的注入方式

      ```java
      <bean id="HelloWorld" class="top.wnghilin.test.HelloWorld">
              <property name="name" value="Spring"></property>
      </bean>
      ```

  * 构造方法注入：

    ```java
    <bean id="car" class="top.wnghilin.test.Car">
            <constructor-arg value="Audi" index="0" type="java.lang.String"></constructor-arg>
            <constructor-arg value="ShangHi" index="1" type="java.lang.String"></constructor-arg>
            <constructor-arg value="6000000" index="2" type="int"></constructor-arg>
    </bean>
    ```

    * index为构造器中参数的编号，若不指定index属性，则默认按照参数顺序进行注入
    * type属性：指定参数的类型，用于区分重载的构造器



#### 属性注入的一些细节

- 用value属性或value标签实现。如果有特殊字符，则用&lt;![CDATA[]]>包裹起来

  ```xml
  <value><![CDATA[<上海>]]></value>
  ```

- 可以用&lt;null/>标签给某个属性注入null值

* 引用其它的Bean：

  * 可以使用property的ref属性建立bean之间的引用关系（或&lt;ref>属性）

    ```xml
    <property name="car" ref="car2"></property>
    ```

  * 可以在Bean内部创造一个bean（内部Bean不能被外部引用）

    ```xml
    <bean id="Person" class="top.wnghilin.test.Person">
        <property name="name" value="Ford"></property>
        <property name="age" value="18"></property>
        <property name="car">
            <bean class="top.wnghilin.test.Car">
                <constructor-arg index="0" value="Fox"></constructor-arg>
                <constructor-arg value="北京" index="1"></constructor-arg>
                <constructor-arg value="200" index="2"></constructor-arg>
            </bean>
        </property>
    </bean>
    ```

* Spring支持级联属性的配置：

  ```xml
  <constructor-arg ref="car"></constructor-arg>
  <!-- 为级联属性赋值 -->
  <property name="car.maxSpeed" value="250"></property>
  ```

  * **注意**：属性需要先初始化后才可以为级联属性赋值

* 集合属性：如果某个属性是集合类型的，用list元素包起来(数组也用list标签)

  ```xml
  <bean id="Person" class="top.wnghilin.collection.Person">
      <property name="name" value="Fox"></property>
      <property name="age" value="12"></property>
      <property name="cars">
          <list>
              <ref bean="car"/>
              <ref bean="car2"/>
              <ref bean="car3"/>
          </list>
      </property>
  </bean>
  ```

  * Set类型的属性需要用set标签

* Map属性：用map标签包裹，entry标签指定值

  ```xml
  <property name="cars">
      <map>
          <entry key="AA" value-ref="car"></entry>
          <entry key="BB" value-ref="car2"></entry>
      </map>
  </property>
  ```

* 使用props标签定义java.util.Properties，该标签使用多个prop作为子标签，每个prop标签必须定义key属性

* 配置单例的集合bean，以供多个bean进行引用，需要导入util命名空间

  ```xml
  <util:list id="cars">
          <ref bean="car2"/>
          <ref bean="car"/>
          <ref bean="car3"/>
      </util:list>
  
      <bean id="person4" class="top.wnghilin.collection.Person">
          <property name="name" value="Spring"></property>
          <property name="age" value="29"></property>
          <property name="cars" ref="cars"></property>
      </bean>
  ```

* 使用p命名空间为bean的属性赋值：

  * 导入p命名空间
  * 使用p命名空间进行赋值

  ```xml
  <bean id="person5" class="top.wnghilin.collection.Person"
        p:name="spring" p:age="30" p:cars-ref="cars">
  </bean>
  ```




* 自动装配：

  * **IOC**容器可以自动装配Bean，需要做的就是在&lt;bean>的autowire属性里指定自动装配的模式

    * **byType**（根据类型自动装配）：根据bean的类型和当前bean的属性的类型进行自动装配。如果有多个类型一致的Bean，无法自动装配
    * **byName**（根据类型名称自动装配）：根据bean的名字和当前bean的setter风格属性名进行自动装配，若没有匹配的，则不装配。
    * constructor：不推荐使用

    ```xml
    <bean id="person" class="top.wnghilin.autowire.Person"
        p:name="Tom" autowire="byName"></bean>
    ```

  * 缺点：不够灵活，实际开发中很少使用自动装配功能



#### Bean之间的关系

1. 继承：使用parent属性指定继承哪个bean的配置，子bean可以覆盖父bean的配置
   * 如果只想把父bean当作模板，可以把abstract属性设为true，这样其就不会被实例化
2. 依赖：设置depends-on属性



#### Bean的作用域

1. 使用scope属性设置作用于
   * singleton：单例的，默认值。容器初始时创建bean实例，在整个容器的生命周期内只创建这一次
   * prototype：原型的，每次向容器请求bean，都会产生一个新的bean，容器初始化时不创建bean的实例。



#### 使用外部属性文件

* Spring提供一个PropertyPlaceholderConfigurer的BeanFactory后置处理器，这个处理器允许用户将Bean配置的部分内容外移到属性文件中，可以在Bean配置文件里使用形式为 **${var}** 的变量，该处理器从属性文件加载属性，并使用这些属性来替换变量。
* Spring还允许在属性文件中使用 **${propName}**，以实现属性之间的相互引用。                                                         



* 使用步骤：

  1. 导入属性文件：<context:property-placeholder location="classpath:db.properties"/>

  2. 使用外部化属性文件的属性

     ```xml
     <bean id="DB" class="top.wnghilin.properties.DB">
         <property name="user" value="${user}"></property>
         <property name="password" value="${password}"></property>
         <property name="driverclass" value="${driverclass}"></property>
         <property name="jdbcurl" value="${jdbcurl}"></property>
     </bean>
     ```



### 工厂方法

* 静态工厂方法

  * 设置静态工厂方法

    ```java
    /**
     * 静态工厂方法：直接调用某一个类的静态方法就可以返回Bean的实例
     */
    public class StaticCarFactory {
        private static Map<String, Car> cars = new HashMap<String, Car>();
    
        static {
            cars.put("Audi", new Car("audi", 300000));
            cars.put("Ford", new Car("Ford", 400000));
        }
        //静态工厂方法
        public static Car getCar(String name){
            return cars.get(name);
        }
    }
    ```

  * 配置静态工厂方法

    ```xml
    <bean id="car1"
          class="top.wnghilin.factory.StaticCarFactory" 
          factory-method="getCar">
    
        <constructor-arg value="audi"/>
    </bean>
    ```

    * factory-method：指向静态工厂方法的名字
    * constructor-arg：如果工厂方法需要传入参数，则使用该标签，来配置参数

* 实例工厂方法

  * 设置实例工厂方法

    ```java
    /**
     * 实例工厂方法：实例工厂的方法，即先需要创建工厂本身，再调用工厂的实例方法来返回bean的实例
     */
    public class InstanceCarFactory {
        private Map<String, Car> cars = null;
    
        public InstanceCarFactory(){
            cars = new HashMap<String, Car>();
            cars.put("Audi", new Car("Audi", 500000));
            cars.put("Ford", new Car("Ford", 400000));
        }
    
        public Car getCar(String brand){
           return cars.get(brand);
        }
    }
    ```

  * 配置实例工厂方法

    ```xml
    <!--配置工厂的实例-->
    <bean id="carFactory" class="top.wnghilin.factory.InstanceCarFactory"></bean>
    
    <!--通过实例工厂方法来配置bean-->
    <bean id="car2" factory-bean="carFactory" factory-method="getCar">
        <constructor-arg value="Ford"></constructor-arg>
    </bean>
    ```



### FactoryBean

* 设置FactoryBean

  ```java
  public class CarFactoryBean implements FactoryBean {
      private String brand;
  
      public String getBrand() {
          return brand;
      }
  
      public void setBrand(String brand) {
          this.brand = brand;
      }
  
      //返回Bean的对象
      @Override
      public Object getObject() throws Exception {
          return new Car(this.brand, 1000000);
      }
      //返回Bean的类型
      @Override
      public Class<?> getObjectType() {
          return Car.class;
      }
  
      //返回是不是单实例的
      @Override
      public boolean isSingleton() {
          return true;
      }
  }
  ```

* 配置FactoryBean

  ```xml
  <bean id="car" class="top.wnghilin.factoryBean.CarFactoryBean">
      <property name="brand" value="BMW"></property>
  </bean>
  ```

* 实际返回的实例是FactoryBean的getObject方法返回的Bean

### 基于注解

* **组件扫描**：Spring能够从classpath下自动扫描，侦测和实例化具有特定注解的组件

* 特定组件有：
  * @Component：基本注解，标识了一个受Spring管理的组件
  * @Respository：标识持久层组件
  * @Service：标识服务层（业务层）组件
  * @Controller：标识表现层组件
  
* 对于扫描到的组件，Spring有默认的命名策略：使用非限定类名，第一个字母小写，也可以再注解中通过value属性值标识组件的名称

* 当在组件类上使用了特定的注解之后，还需要在Spring的配置文件中声明&lt;context:component-scan>

  * base-package属性指定一个需要扫描的基类包，Spring容器会扫描该包及其子包中的所有类

  * 扫描多个包时，用逗号分隔

  * 用resource-pattern来过滤特定的类

  * Bean中属性的值可以用**@Value()**指定，或者使用**@Autowired**，自动装配。根据类型来检测扫描容器当中符合这个属性类型的对象，如果检测到了，找到这个对象，赋值给这个属性。也可以使用**@Qualifier**指定装配特定的bean
  
    ```xml
    <context:component-scan 
                            base-package="top.wnghilin.spring.beans",
                          resource-pattern="autowire/*.class"/>
    ```

  * <context:include-filter&gt;子节点表示要包含的目标类

  * <context:exclude-filter&gt;子节点表示要排除在外的目标类

    * 过滤表达式内容：

      * annotation：所有标注了XXXAnnotation的类

      * assinable：所有继承或扩展了XXXService的类

      * 注意将bean 的use-default-filter属性设置为false
  
        ```xml
      <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Repository"/>
        ```

        
  
    ```java
    @Service
    public class UserService {
        public void  add(){
            System.out.println("UserService add...");
        }
    }
    
    @Repository("userRepository")
    public class UserRepositoryImpl implements UserRepository{
        @Override
        public void save() {
            System.out.println("User Repository save.....");
        }
  }
    ```
  
    ```xml
  <context:component-scan base-package="top.wnghilin.annotation"></context:component-scan>
    ```
  



* **泛型依赖注入**：了解

***

## SpEL

* **Spring表达式语言**（简称SpEL）：是一个支持运行时查询和操作对象图的强大的表达式语言

* 通过SpEL可以实现
  * 通过bean的id对bean进行引用
  * 调用方法以及引用对象中的属性
  * 计算表达式的值
  * 正则表达式的匹配
  
* 字面量

  * 整数：&lt;property name="count" value="#{5}"/>
  * 小数：&lt;property name="frequency" value="#{89.7}"/>
  * 科学记数法：&lt;property name="count" value="#{1e4}"/>
  * String可以使用单引号或者双引号作为字符串的定界符号：
    * &lt;property name="count" value="#{'Chuck'}"/>或&lt;property name="count" value='#{"Chuck"}'/>
  * 布尔值：&lt;property name="enabled" value="#{false}"/>

* 其它作用

  * 引用其它对象：除了使用ref，也可以使用value
    * &lt;property name="prefix" value="#{prefixGenerator}"/>（括号内为其它的Bean）
  * 引用其他对象的属性
    * &lt;property name="suffix" value="#{sequenceGenerator.suffix}"/>
  * 调用其它方法，还可以链式操作
    * &lt;property name="suffix" value="#{sequenceGenerator.toString().toUpperCase()}"/>

* 支持的运算符号

  * 算数运算符：+  -  *  /  %  ^
  * 加号可以用作字符串连接：&lt;property name="count" value="#{firstname+' '+lastname}"/>
  * 比较运算符：&gt;  &lt;  ==  &lt;=  &gt;=  lt  gt  eq  le  ge
  * 逻辑运算符：and  or  not
  * if-else运算符：?(temary):(Elvis)
  * 正则表达式：matches
  * 调用静态方法或静态属性

  <center>使用示例</center>

```xml
<bean id="address" class="top.wnghilin.spel.Address">
    <property name="city" value="#{'Beijing'}"></property>
    <property name="street" value="WuDaokou"></property>
</bean>

<bean id="car" class="top.wnghilin.spel.Car">
    <property name="brand" value="Audi"></property>
    <property name="price" value="#{500000.0}"></property>
    <!--使用SpEL引用类的静态属性计算轮胎周长-->
    <property name="tyrePerimeter" value="#{T(java.lang.Math).PI * 80}"></property>
</bean>

<!--使用SpEL引用其它的Bean及其属性-->
<bean id="person" class="top.wnghilin.spel.Person">
    <property name="name" value="Tom"></property>
    <property name="city" value="#{address.city}"></property>
    <property name="car" value="#{car}"></property>
    <!--在SpEL中使用运算符-->
    <property name="info" value="#{car.price > 300000 ? '金领' : '白领'}"></property>
</bean>
```



## IOC容器中Bean的生命周期

* IOC容器对Bean生命周期的管理过程：
  * 通过构造器或工厂方法创建Bean实例
  * 为Bean的属性设置值和对其它Bean的引用
  * 调用Beand的初始化方法
  * Bean可以使用
  * 当容器关闭时，调用Bean的销毁方法
* 在Bean的声明里设置init-method和destroy-method属性，为Bean指定初始化和销毁方法



* Bean的后置处理器允许在调用初始化方法前后对Bean进行额外的处理
* 后置处理器需要实现BeanPostProcessor接口，并在xml文件中进行配置
  * public Object postProcessAfterInitialization(Object bean, String beanName)
  * public Object postProcessBeforeInitialization(Object bean, String beanName)
    * bean：实例本身
    * beanName：IOC容器配置的bean的名字
    * 返回值：是实际上返回给用户的Bean，可以在以上两个中修改返回的bean，甚至返回一个新的bean
* 配置：不需要配置id，IOC容器自动识别是一个BeanPostProcessor



## AOP

假设我们实现一个加减乘除的类，要求是在每一次运算都加上日志和验证，如果在这个类的每一个方法都加入以上业务无关代码，可能会导致代码冗余，不利于维护。

* 使用**动态代理**解决日志、验证等非业务需求导致的代码冗余等问题

  * 动态代理：通过代理来拦截对真实业务对象的访问

  * java.lang.reflect.Proxy类：要想使用动态代理，需要通过这个类创建一个代理对象。

    * ClassLoader loader用来指明生成代理对象使用哪个类装载器，Class<?>[] interfaces用来指明生成哪个对象的代理对象，通过接口指定，InvocationHandler h用来指明产生的这个代理对象要做什么事情

  * 步骤

    * 定义一个业务对象的接口

      ```java
      public interface Calculator {
          int add(int i, int j);
          int sub(int i, int j);
          int div(int i, int j);
          int mul(int i, int j);
      }
      ```

    * 定义目标业务对象类

      ```java
      public class CalculatorImpl implements Calculator{
          @Override
          public int add(int i, int j) {
              return i + j;
          }
      
          @Override
          public int sub(int i, int j) {
              return i - j;
          }
      
          @Override
          public int div(int i, int j) {
              return i / j;
          }
      
          @Override
          public int mul(int i, int j) {
              return i * j;
          }
      }
      ```

    * 创建生成代理对象的代理类

      ```java
      public class CalculatorProxy {
          //要代理的类
          private Calculator target;
      
          public CalculatorProxy(Calculator target) {
              this.target = target;
          }
      
          public Calculator getLogProxy(){
              //代理对象由类加载器加载
              ClassLoader loader = target.getClass().getClassLoader();
              //代理对象类型，即其中有哪些方法
              Class [] interfaces = new Class[]{Calculator.class};
              //代理要执行什么
              InvocationHandler h = new InvocationHandler() {
                  /**
                   * 执行代码
                   * @param proxy:正在返回的那个代理对象, invoke方法中一般不使用
                   * @param method:正在调用的方法
                   * @param args:传入的参数
                   * @throws Throwable
                   */
                  @Override
                  public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                      System.out.println("invoke.......");
                      //日志
                      String methodName = method.getName();
                      System.out.println("The method " + methodName + " begin with" + Arrays.asList(args));
      
                      //执行方法
                      Object result = method.invoke(target, args);
      
                      //日志
                      System.out.println("The method " + methodName + " ends with" + result);
                      return result;
                  }
              };
      
              Calculator proxy = (Calculator) Proxy.newProxyInstance(loader, interfaces, h);
              return proxy;
          }
      }
      ```

      

* Spring的AOP可以更简单地实现以上功能
* **AOP**：面向切面编程
  * AOP的主要对象是切面
  * 如以上问题，日志可以看作一个切面，验证可以看作一个切面
* **术语**：
  * 切面(Aspect)：横切关注点（跨越应用程序多个模块的功能）被模块化的特殊对象
  * 通知(Advice)：切面必须要完成的工作
  * 目标(Target)：被通知的对象
  * 代理(Proxy)：向目标对象应用通知之后创建的对象
  * 连接点(JoinPoint)：程序执行的某个特定位置
  * 切点(pointcut)：如果把连接点当作数据库中的记录，切点就相当于查询条件

### 基于注解方式配置AOP

* 使用**AspectJ**实现AOP：

* 步骤：

  1. 导入jar包：[https://mvnrepository.com/artifact/org.aspectj/aspectjweaver](https://mvnrepository.com/artifact/org.aspectj/aspectjweaver)

  2. 创建IOC容器

  3. 创建实现AOP的类，如LoggingAspect

  4. 在LoggingAspect上添加@Component注解和@Aspect注解

     ```java
     @Aspect
     @Component
     public class LoggingAspect
     ```

  5. 在LoggingAspect种实现方法，并指定其在什么时候起作用（**切点表达式**），execution中可以使用*来表示任意，**..**表示参数任意

     ```java
     @Before("execution(public int wnghilin.top.AOP.Calculator.*(int, int))")
     public void BeforeMethod()
     ```

  6. 配置SpringXML文件，使其自动生成代理

     ```xml
     <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
     ```

  7. 如果想要关于改方法的具体信息，可以在BeforeMethod方法中添加Joinpoint类型的参数获取参数列表和方法签名

     ```java
     @Before("execution(public int wnghilin.top.AOP.Calculator.add(int, int))")
     public void BeforeMethod(JoinPoint joinPoint){
         String methodName = joinPoint.getSignature().getName();
         List<Object> args = Arrays.asList(joinPoint.getArgs());
         System.out.println("The method" + methodName + " is used" + ", args: " + args);
     }
     ```

  8. 完整代码：

     ```java
     //Main方法
     public class Main {
         public static void main(String[] args) {
             ApplicationContext ctx = new ClassPathXmlApplicationContext("beans-AOP.xml");
     
             Calculator cal = (Calculator)ctx.getBean(Calculator.class);
     
             int result = cal.add(1, 3);
     
             System.out.println("result = " + result);
         }
     }
     
     //LoggingAspect
     @Aspect
     @Component
     public class LoggingAspect {
         @Before("execution(public int wnghilin.top.AOP.Calculator.*(int, int))")
         public void BeforeMethod(JoinPoint joinPoint){
             String methodName = joinPoint.getSignature().getName();
             List<Object> args = Arrays.asList(joinPoint.getArgs());
             System.out.println("The method" + methodName + " is used" + ", args: " + args);
         }
     }
     
     //xml配置
     <context:component-scan base-package="wnghilin.top.AOP"></context:component-scan>
     <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
     ```



* **通知**：

  * 前置通知：@Before，在目标方法开始之前执行

  * 后置通知：@After，在目标方法结束之后执行（无论是否发生异常），在后置通知中不能访问目标方法执行的结果

  * 返回通知：@AfterReturning，在目标方法**正常**结束之后执行，在返回通知中可以访问目标方法执行的结果

    ```java
    @AfterReturning(value = "execution(public int wnghilin.top.AOP.Calculator.*(int, int))",
                    returning = "result")
    public void afterReturning(JoinPoint joinPoint, Object result){
        String methodName = joinPoint.getSignature().getName();
        List<Object> args = Arrays.asList(joinPoint.getArgs());
        System.out.println("The method " + methodName + " ends with " + result);
    }
    ```

  * 异常通知：@AfterThrowing，可以访问到方法出现的异常，也可以指定发生某个特定异常后执行

    ```java
    @AfterThrowing(value = "execution(public int wnghilin.top.AOP.Calculator.*(int, int))",
                   throwing = "exception")
    public void afterThrowing(JoinPoint joinPoint, Exception exception){
        String methodName = joinPoint.getSignature().getName();
        List<Object> args = Arrays.asList(joinPoint.getArgs());
        System.out.println("The method " + methodName + " occurs exception " + exception);
    }
    ```

  * 环绕通知：@Around，类似于动态代理的全过程

    ```java
    @Around("execution(public * wnghilin.top.AOP.Calculator.*(..))")
    public Object aroundMethod(ProceedingJoinPoint proceedingJoinPoint) {
        Object result = null;
    
        String methodName = proceedingJoinPoint.getSignature().getName();
        List<Object> args = Arrays.asList(proceedingJoinPoint.getArgs());
    
        try {
            //前置通知
            System.out.println("The method " + methodName + " begins with" + args);
            result = proceedingJoinPoint.proceed();
            //返回通知
            System.out.println("The method " + methodName + " ends with" + result);
        } catch (Throwable throwable) {
            //异常通知
            System.out.println("The method " + methodName + " occurs exception " + throwable);
        }
        //后置通知
        System.out.println("The method " + methodName + " ends");
        return result;
    }
    }
    ```



* 指定切面的优先级：@Order(整数值)，值越小，优先级越高

* 重用切点表达式：

  ```java
  /**
  * 声明一个方法声明切面表达式，该方法中不需要任何代码
  */
  @Pointcut("execution(public int wnghilin.top.AOP.Calculator.*(int, int))")
  public void declareJoinPointExpression(){}
  
  //引用
  @After("declareJoinPointExpression()")
  ```

  

### 基于配置文件方式配置AOP

1. 配置Bean

   ```xml
   <bean id="calculator" class="wnghilin.top.XML.CalculatorImpl"></bean>
   ```

2. 配置切面的Bean

   ```xml
   <bean id="loggingAspect" class="wnghilin.top.XML.LoggingAspect"></bean>
   ```

3. 配置AOP

   ```XML
   <!--配置AOP-->
   <aop:config>
       <!--配置切点表达式-->
       <aop:pointcut id="pointCut" expression="execution(* wnghilin.top.XML.CalculatorImpl.*(..))"/>
       <!--配置切面-->
       <aop:aspect ref="loggingAspect" order="1">
           <aop:before method="BeforeMethod" pointcut-ref="pointCut"></aop:before>
           <aop:after method="AfterMethod" pointcut-ref="pointCut"></aop:after>
           <aop:after-returning method="afterReturning" pointcut-ref="pointCut" returning="result"></aop:after-returning>
           <aop:after-throwing method="afterThrowing" pointcut-ref="pointCut" throwing="exception"></aop:after-throwing>
       </aop:aspect>
   </aop:config>
   ```

   



## 使用JDBCTemplate

### JDBC的配置

1. 配置数据源（以C3P0为例）

   ```xml
   <context:property-placeholder location="classpath:db.properties"></context:property-placeholder>
   ```

2. 配置相关参数

   ```xml
   <bean id="dataSource"
         class="com.mchange.v2.c3p0.ComboPooledDataSource">
       <property name="user" value="${jdbc.user}"></property>
       <property name="password" value="${jdbc.password}"></property>
       <property name="driverClass" value="${jdbc.driverClass}"></property>
       <property name="jdbcUrl" value="${jdbc.url}"></property>
       <property name="initialPoolSize" value="${jdbc.initPoolSize}"></property>
       <property name="maxPoolSize" value="${jdbc.maxPoolSize}"></property>
   </bean>
   ```

3. 配置JDBCTemplate

   ```xml
   <bean id="jdbcTemplate"
         class="org.springframework.jdbc.core.JdbcTemplate">
       <property name="dataSource" ref="dataSource"></property>
   </bean>
   ```

   此时已经可以使用JDBC进行数据库操作



* JDBCTemplate的一些操作

  * 批量更新：batchUpdate(sql, batchArgs) 此处batchArgs是Object[] 的List类型

    ```java
    @Test
    public void TestBatchUpdate(){
        String sql = "INSERT INTO student(age, name, math) VALUES(?, ?, ?)";
    
        List<Object[]> batchArgs = new ArrayList<>();
    
        batchArgs.add(new Object[]{11, "aa", 34.5});
        batchArgs.add(new Object[]{13, "bb", 23.5});
        batchArgs.add(new Object[]{12, "cc", 34.5});
        batchArgs.add(new Object[]{16, "dd", 34.5});
    
        jdbcTemplate.batchUpdate(sql, batchArgs);
    }
    ```

  * 从数据库中获取一条记录，并返回一个对象：queryForObject

    ```java
    @Test
    public void TestQueryForObject(){
        String sql = "SELECT id, age, name, math FROM student WHERE id=?";
        Student stu = null;
        RowMapper<Student> rowMapper = new BeanPropertyRowMapper<>(Student.class);
        stu = jdbcTemplate.queryForObject(sql, rowMapper, 1);
        System.out.println(stu);
    }
    ```

    

### 在JDBC中使用具名参数

* 好处：若有多个参数，则不用再去对应位置，直接对应参数名，便于维护
* 缺点：较为麻烦

1. 配置

   ```xml
   <!--配置NamedParameterJdbcTemplate对象，该对象可以使用具名参数，其没有无参数的构造器，所以必须为其构造器指定参数-->
   <bean id="namedParameterJdbcTemplate"
     class="org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate">
       <constructor-arg ref="dataSource"/>
   </bean>
   ```

   

2. 使用

   方法一：使用Map封装

   ```java
   @Test
   public void TestNamedParameter(){
       String sql = "INSERT INTO student(id, age, name, math) VALUES(:id, :age, :name, :math) ";
       Map<String, Object> paraSource = new HashMap<>();
       paraSource.put("id", 7);
       paraSource.put("age", 18);
       paraSource.put("name", "wangwei");
       paraSource.put("math", 67.8);
   
       namedParameterJdbcTemplate.update(sql, paraSource);
   }
   ```

   方法二：使用对象封装

   ```java
   @Test
   public void TestNamedParameter(){
       String sql = "INSERT INTO student(id, age, name, math) VALUES(:id, :age, :name, :math) ";
       Student student = new Student();
       student.setAge(8);
       student.setId(8);
       student.setName("zhanglei");
       student.setMath(89.8);
   
       SqlParameterSource paraSource = new BeanPropertySqlParameterSource(student);
   
       namedParameterJdbcTemplate.update(sql, paraSource);
   }
   ```



## 事务管理

Spring采用**声明式事务管理**，使用事务管理器（TransactionManager）进行事务管理

* 配置

  ```xml
  <!--配置事务管理器-->
  <bean id="transactionManager" 
        class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
  	<property name="dataSource" ref="dataSource"></property>
  </bean>
  
  <!--启用事务注解-->
  <tx:annotation-driven transaction-manager="transactionManager"/>
  ```

* 在需要使用事务的方法上添加@Transactional注解

### 事务传播属性

* 定义：当事务方法被另一个事务方法调用时，必须指定事务应该如何传播，如方法是在现有事务中还是新开启一个事务

* Spring支持的事务传播行为：

  * **REQUIRED**：**默认行为**，如果有事务在运行，当前的方法就在这个事务内运行，否则，就启动一个新的事务，并在自己的事务内运行
  * REQUIRES_NEW：当前的方法必须启动新事务，并在它自己的事务内运行，如果有事务正在运行，应该将它挂起

* 事务传播属性可以在@Transaction注解的propagation属性中定义

  * ```java
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    ```



### 其他属性

1. 隔离级别：使用isolation指定事务的隔离级别，最常用的是READ_COMMITTED
   * isolation = Isolation.READ_COMMITTED
2. 回滚：默认情况下Spring的声明式事务对所有的运行时异常进行回滚，也可以通过rollbackFor属性设置什么时候进行回滚或noRollbackFor设置什么时候不回滚
   * noRollbackFor = {RuntimeException.class}
3. 使用readOnly指定事务是否为只读，可以帮助数据库引擎优化事务，若真的是一个只读取数据库的事务，应设置为true
4. 使用timeout指定强制回滚之前事务可以占用的时间，可以防止事务一直占用



### 使用XML进行事务管理

1. 配置Bean

2. 配置事务管理器

   ```xml
   <bean id="" 
         class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
   <property name-"dataSource" ref="dataSource"></property>
   </bean>
   ```

3. 配置事务属性

   ```xml
   <tx:advice id="txAdvice" transaction-manager="transactionManager">
   	<tx:attributes>
           <tx:method name="方法名(可用*代表任意)" propagation="REQUIRES_NEW"/>
           <tx:method name="get*" read-only="true"/>
           <tx:method name="find*" read-only="true"/>
       </tx:attributes>
   </tx:advice>
   ```

4. 配置事务切入点，以及吧事务切入点和事务属性关联起来

   ```xml
   <aop:config>
   	<aop:pointcut expression="execution(修饰符+全类名+方法名)" id="txPointCut"/>
       <aop:advisor acvice-ref="txAdvice" pointcut-ref="txPointCut"/>
   </aop:config>
   ```

   

