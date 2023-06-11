1. Spring环境搭建

  1. Spring Jar参照

     ```markdown
      <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
     <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.1.4.RELEASE</version>
      </dependency>
     ※From mvnrepository.com
     ```
  2. Spring 配置文件作成
     - 配置文件放置位置  任意位置
     - 配置文件命名 建议: applicationContext.xml
     - 日后应用spring框架时 需要配置文件路径的设置

2. 注意点

  1. Spring工厂是可以调用对象的私有构造方法来创建对象的
  2. 实体对象(Entity)会封装数据库中表的数据, 不是由Spring工厂来创建,  而是交给持久层框架来创建(Mybatis,Hibernate,JPA)

3. Spring5.x与日志框架的整合

  1. Spring与日志框架整合, 日志框架就可以在控制台中, 输出Spring运行过程中的重要信息。

  2. 好处: 便于了解Spring框架的运行过程, 利于程序的调试。

  3. Spring如何整合日志框架

     ~~~markdown
     默认
     1. Spring1,2,3早期版本与commons-logging.jar进行整合的
     2. Spring4,5.x 默认整合的日志框架 logback log4j2
     
     Spring5.x整合log4j
       1. 引入log4j jar包(※引入slf4j是为了不使用默认的logback log4j2)
       2. 引入log4j.properties配置文件
     ~~~

     - pom

       ~~~markdown
       <dependency>
           <groupId>org.slf4j</groupId>
           <artifactId>slf4j-log4j12</artifactId>
           <version>1.7.25</version>
       </dependency>
       <dependency>
           <groupId>log4j</groupId>
           <artifactId>log4j</artifactId>
           <version>1.2.17</version>
       </dependency>
       ~~~
       
     - log4j.properties
       
       ~~~markdown
       # resources文件夹根目录下
       ### 配置根
       log4j.rootLogger = debug,console
       
       ### 日志输出到控制台显示
       log4j.appender.console = org.apache.log4j.ConsoleAppender
       log4j.appender.console.Target = System.out
       log4j.appender.console.layout = org.apache.log4j.PatternLayout
       log4j.appender.console.layout.ConversionPattern = %d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n
       ~~~

4. 注入(Injection)

   1. 什么是注入

      ~~~markdown
      通过Spring工厂及配置文件, 为所创建对象的成员变量赋值
      ~~~

   2. 如何注入

      - 类成员变量提供set,get方法

      - 配置spring的配置文件

        ~~~markdown
            <bean id="person" class="com.ws.pattern.factory.Person">
                <property name="id">
                    <value>1</value>
                </property>
                <property name="name">
                    <value>wensu</value>
                </property>
            </bean>
        ~~~

   3. 注入好处: 解耦合

5. Set注入详解

   ~~~markdown
   针对不同类型的成员变量, 在<property>标签中需要嵌套其他标签
   ~~~

   ![image-20230611214546602](https://github.com/wensugithub/markdownPicture/blob/main/image-20230611214546602.png?raw=true)

   - JDK内置类型

   ​       

   

   

   

   - 用户自定义类型

6. aa

7. 