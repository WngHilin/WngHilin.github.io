---
title: JavaWeb基础概念
date: 2019-07-27 15:25:49
tags: 
- Javaweb
- 学习
- 反射
- 注解
categories: JavaWeb
---

## 反射

> 框架：**软件框架**（software framework），通常指的是为了实现某个业界标准或完成特定基本任务的[软件组件](https://baike.baidu.com/item/软件组件/9817461)规范，也指为了实现某个软件组件规范时，提供规范所要求之基础功能的软件产品。也被称为半成品软件。可以在框架的基础上进行软件开发，可以简化编码。

### 概念

 * 反射：将类的各个组成部分封装为其他对象，这就是反射机制。（在Class 类对象阶段，将.class文件的各个部分封装成不同的类对象，如将成员变量封装成Field类对象并用数组存储起来。其总体是由Class类对象储存的）
    * 好处：
      	* 1.可以在程序运行过程中操作这些对象。
      	* 2.可以解耦

### 获取字节码class对象的三种方式

* 获取Class对象的方式：
	1. Class.forName("全类名")：将字节码文件加载进内存，返回Class对象
		
		* 多用于配置文件，将类名定义在配置文件中。读取文件，加载类
		
	2. 类名.class：通过类名的属性class获取
		
	* 多用于参数的传递
		
	3. 对象.getClass()：getClass()在Object类中定义 
		
		* 多用于对象的获取字节码的方式
		
	
	* 结论：
		同一个字节码文件(*.class)在一次程序运行过程中，只会被加载一次，不论通过哪一种方式获取的Class对象都是同一个。

### class类对象的获取功能

* Class对象功能：
	* 获取功能：
		1. 获取成员变量：
			* getFields() ：获取所有public修饰的成员变量，private或protected修饰的无法读取
			* getField(String name)   获取指定名称的 public修饰的成员变量
			* getDeclaredFields()  获取所有的成员变量，不考虑修饰符
			* getDeclaredField(String name)  
			
			```java
				//用法示例
				  public class reflectDemo {
				      public static void main(String[] args) {
				          Class<Person> personClass = Person.class;
				          Field[] fields = personClass.getFields();
			        for (Field field : fields) {
				              System.out.println(field);
				          }
			        Field a = personClass.getField("a");
				      }
				  }
			```
			
			
			
			成员变量的操作（Field类的方法）
			
			1. 设置值
			
			   - void set(Object obj, Object value)  
			
			   ```java
			   Person p = new Person();
			   a.set(p, "a"); //设置p对象的a属性的值
			   ```
			
			2. 获取值
			
			   - get(Object obj) 
			
			3. 忽略访问权限修饰符的安全检查
			
			   * setAccessible(true):暴力反射，设置之后即可访问private和protected修饰的变量
			
		2. 获取构造方法：
		
		   * getConstructors()  
		   * getConstructor(类<?>... parameterTypes)  
		   * getDeclaredConstructor(类<?>... parameterTypes)  
		   * getDeclaredConstructors()  
		
		   ```java
		   public class reflect {
		       public static void main(String[] args) throws Exception {
		           Constructor cons = cls2.getConstructor(String.class, int.class);
		           System.out.println(cons);
		       }
		   }
		   ```
		
		   * Constructor对象的操作：
		     * 创建对象：
		       * newInstance方法
		       * 用空参数构造方法创建对象，可以用Class对象的newInstance方法
		     * 暴力反射
		
		3. 获取成员方法：
		
		   * Method[] getMethods()  Method getMethod(String name, 类<?>... parameterTypes)
		   * Method[] getDeclaredMethods() 
		   * Method getDeclaredMethod(String name, 类<?>... parameterTypes) 
		   * 操作：
		     * 执行方法：invoke方法，参数为一个对象
		     * 获取方法名称：getname
		
		4. 获取全类名	
		
		   * String getName()
	

### 实例

* 案例：
	* 需求：写一个"框架"，不能改变该类的任何代码的前提下，可以帮我们创建任意类的对象，并且执行其中任意方法
		* 实现：
			1. 配置文件
			2. 反射
		* 步骤：
			1. 将需要创建的对象的全类名和需要执行的方法定义在配置文件中
			2. 在程序中加载读取配置文件
			3. 使用反射技术来加载类文件进内存
			4. 创建对象
			5. 执行方法
	
	```java
	public class reflectTest {
	    public static void main(String[] args) throws Exception{
	        //加载配置文件
	        //1.1创建Properties对象
	        Properties pro = new Properties();
	        //1.2加载配置文件，转换为一个集合
	        //1.2.1获取class目录的配置文件
	        ClassLoader classLoader = reflectTest.class.getClassLoader();
	        InputStream is = classLoader.getResourceAsStream("pro.properties");
	        pro.load(is);
	
	        //2.获取配置文件中的数据
	        String className = pro.getProperty("className");
	        String methodName = pro.getProperty("methodName");
	
	        //3.加载该类进内存
	        Class cls = Class.forName(className);
	
	        //4.创建对象
	        Object obj = cls.newInstance();
	
	        //5.获取方法对象
	        Method method = cls.getMethod(methodName);
	
	        //6.执行方法
	        method.invoke(obj);
	    }
	}
	```
	

***

## 注解

>* 概念：说明程序的。给计算机看的
>* 注释：用文字描述程序的。给程序员看的
>* 定义：注解（Annotation），也叫元数据。一种代码级别的说明。它是JDK1.5及以后版本引入的一个特性，与类、接口、枚举是在同一个层次。它可以声明在包、类、字段、方法、局部变量、方法参数等的前面，用来对这些元素进行说明，注释。
>* 作用分类：
>  ①编写文档：通过代码里标识的注解生成文档【生成文档doc文档】
>  ②代码分析：通过代码里标识的注解对代码进行分析【使用反射】
>  ③编译检查：通过代码里标识的注解让编译器能够实现基本的编译检查【Override】

### 内置注解

* @Override：检测被该注解标注的方法是否是继承自父类（接口）的。
* @Deprecated：该注解标注的内容，已过时
* @SuppressWarnings：压制警告
  * @SuppressWarnings("all")：压制所有警告

### 自定义注解

* 格式：

  * ```java
  元注解
    public @interface 注解名{
  	属性列表
    }
    ```
  
* 本质：

  * ```java
    public interface MyAnno extends java.lang.annotation.Annotation {
    }
    ```

  * 其本质上就是一个接口，该接口默认继承Annotation接口

* 属性：接口中可以定义的成员方法

  * 要求：

    * 属性的返回值类型：
      * 基本数据类型
      * String
      * 枚举
      * 注解
      * 以上类型的数组
    * 定义了属性，使用时需要给属性赋值

    ```java
    //定义
    public @interface MyAnno {
        int age();
        String name() default "张三"
    
        Person person();//此处为枚举类型
        MyAnno2 anno2();//此处为另一个注解
    
        String[] strs();
    }
    
    //使用，有默认值的不需要赋值
    @MyAnno(show1 = 1, person = Person.P1, anno2 = @MyAnno, strs = {"aaa","bbb"})
    public class AnnoDemo1 {}
    ```

    * **特殊情况**：如果只有一个属性需要赋值且属性名是**value**时，可以省略value，即MyAnno(1)。

* 元注解：解释注解的注解

  * @Target：描述注解能够作用的位置
    * ElementType取值：
      * TYPE：可以作用于类上
      * METHOD：可以作用于方法上
      * FIELD：可以作用于成员变量上
  * @Retention：描述注解被保留的阶段
    * @Retention(RetentionPolicy.RUNTIME)：当前被描述的注解，会保留到class字节码文件中，并被JVM读取到
    * 
  * @Documented：描述直接是否被抽取到API文档中
  * @Inherited：描述注解是否被子类继承

  ```java
  import java.lang.annotation.*;
  
  @Target(value={ElementType.TYPE, ElementType.METHOD})
  @Retention(RetentionPolicy.RUNTIME)
  @Documented
  @Inherited
  public @interface MyAnno3 {
  
  }
  ```

### 解析注解

* 解析注解：获取注解中定义的属性值，可以不再使用配置文件

  * 1.获取注解定义的位置的对象（Class，Method，Field）
  * 2.获取指定的注解
    * getAnnotation(Class)

  ​		

```java
//注解定义
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

/**
 * 描述需要执行的类名和方法名
 */
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface Pro {
    String classname();
    String methodname();
}

//实现
import java.lang.reflect.Method;

@Pro(classname = "top.wnghillin.annotation.Student", methodname = "study")
public class reflectTest {
    public static void main(String[] args) throws Exception{
        //1.解析注解
        //1.1获取该类的字节码文件对象
        Class<reflectTest> reflectTestClass = reflectTest.class;
        //2获取注解对象
        Pro an = reflectTestClass.getAnnotation(Pro.class);
        //3.调用注解对象中定义的抽象方法，获取返回值
        String classname = an.classname();
        String methodname= an.methodname();

        //4.加载该类进内存
        Class cls = Class.forName(classname);

        //5.创建对象
        Object obj = cls.newInstance();

        //6.获取方法对象
        Method method = cls.getMethod(methodname);

        //7.执行方法
        method.invoke(obj);
    }
}
```

* 附：isAnnotationPresent(MyAnno.class)：判断该方法（对象、属性）是否被MyAnno注解



***

资料：[黑马JavaWeb入门到精通(idea版)](黑马JavaWeb入门到精通(idea版))

