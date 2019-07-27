---
title: 使用Junit进行单元测试
date: 2019-07-27 10:50:52
tags: 
- Javaweb
- 学习
categories: JavaWeb
---
## 预备知识
* 黑箱测试：不需要输入代码，只看输入输出

* 白箱测试：需要输入代码进行测试

  （其中，使用Junit进行测试是白箱测试 ）

## Junit使用
* 步骤：

* 1. 定义一个测试类

  * ​	建议使用被测试的类名+Test命名
  * ​    可以建立一个新的test包储存测试类

* 2. 定义测试方法，该测试方法可以独立运行：

   * ​    加上@Test，如下方代码

     ```java
         @Test
         public void testAdd(){
          
         }
     ```

* 导入Junit依赖环境

* 使用断言来判断结果: 

  * 判定结果：红色：失败	绿色：成功

  ```java
      @Test
      public void testAdd(){
          Calculator c = new Calculator();
          int result = c.add(1, 2);
          Assert.assertEquals(期望结果，运算结果)(2, result); 	
          //Assert.assertEquals(期望结果，运算结果)
      }
  ```

  失败结果：

  ```
  java.lang.AssertionError: 
  Expected :2
  Actual   :3
  ```

  

### @Before @After方法

```java
	/**
     * 初始化方法
     * 所有测试方法执行之前都将执行这个方法
     */
    @Before
    public void init(){
        System.out.println("init...");
    }

    /**
     * 释放资源方法
     * 所有测试方法执行之后都会自动执行这个方法
     */
    @After
    public void close(){
        System.out.println("close...");
    }
```

