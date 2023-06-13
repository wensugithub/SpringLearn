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

       - Spring工厂是可以调用对象的私有构造方法来创建对象的
       - 实体对象(Entity)会封装数据库中表的数据, 不是由Spring工厂来创建,  而是交给持久层框架来创建(Mybatis,Hibernate,JPA)

3. Spring5.x与日志框架的整合

       - Spring与日志框架整合, 日志框架就可以在控制台中, 输出Spring运行过程中的重要信息。
        
       - 好处: 便于了解Spring框架的运行过程, 利于程序的调试。
        
       - Spring如何整合日志框架

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

   - 什么是注入

      ~~~markdown
      通过Spring工厂及配置文件, 为所创建对象的成员变量赋值
      ~~~

   - 如何注入

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

   - 注入好处: 解耦合

5. Set注入详解

   ~~~markdown
   针对不同类型的成员变量, 在<property>标签中需要嵌套其他标签
   ~~~

   ![image-20230611214546602](https://github.com/wensugithub/markdownPicture/blob/main/image-20230611214546602.png?raw=true)

   - JDK内置类型

     1. String + 8种基本类型

        ~~~markdown
        <value>ws</value>
        ~~~

     2. 数组

        ~~~markdown
        <list>
        	<value>a@gmail.com</value>
        	<value>b@gmail.com</value>
        	<value>c@gmail.com</value>
        </list>
        ~~~

     3. Set集合

        ~~~markdown
        <Set>
        	<value>a@gmail.com</value>
        	<value>b@gmail.com</value>
        	<value>b@gmail.com</value>
        </Set>
        <set><ref bean></set>
        ※Set无序, 重复的会被过滤掉
        ※Set中不是只可以写value 取决于对象实例成员变量泛型，例如: Set<String> tel；
        ~~~

     4. List集合

        ~~~markdown
        <list>
        	<value>a@gmail.com</value>
        	<value>b@gmail.com</value>
        	<value>c@gmail.com</value>
        </list>
        <list><ref bean></list>
        ※有序, 允许重复
        ※List中不是只可以写value 取决于对象实例成员变量泛型，例如: List<String> tel；
        ~~~

     5. Map集合

        ~~~markdown
        <map>
        	<entry>
        		<key><value>ws</value></key>  -> value标签是由于 对象成员变量是Map<String, String>
        		<value>123456</value>         -> value标签是由于 对象成员变量是Map<String, String>
        	</entry>
        	<entry>
        		<key><ref bean="" /></key> 
        		<ref bean="" />
        	</entry>
        </map>
        ~~~

     6. Properties

        ~~~markdown
        Properties类型 特殊的Map key=String value=String
        <props>
        	<prop key="key1">value1</prop>
        	<prop key="key2">value2</prop>
        </props>
        ~~~

     7. 复杂的JDK类型(Date)

        ~~~markdown
        需要自定义类型转换器来处理
        ~~~


   - 用户自定义类型

     - 第一种方式

       ~~~ markdown
       1. 为成员变量提供set,get方法
       2. 配置文件中进行注入
           <bean id="userService" class="com.ws.pattern.factory.UserServiceImpl">
               <property name="userDAO">
                   <bean class="com.ws.pattern.factory.UserDAOImpl" />
               </property>
           </bean>
           ※此处dao没有被别处引用，所以bean中不需要写id属性
       ~~~

     - 第二种方式

       ~~~markdown
       1. 上面第一种方式配置文件代码冗余 (orderService也调用userDao的时候,userDAO就会重复写一次)
       2. 而且上面的写法,userDAO会重复创建对象, 浪费JVM内存资源
       ~~~

       ~~~markdown
       1. 为成员变量提供set,get方法
       2. 配置文件中进行注入
       <bean id="userDAO" class="com.ws.pattern.factory.UserDAOImpl"/>
       <bean id="userService" class="com.ws.pattern.factory.UserServiceImpl">
           <property name="userDAO">
           	<ref bean="userDAO"/>
           </property>
       </bean>
       <bean id="orderService" class="com.ws.pattern.factory.OrderServiceImpl">
           <property name="userDAO">
           	<ref bean="userDAO"/>
           </property>
       </bean>
       
       ※Spring4.x 废除了 <ref local=""/> 基本等效 <ref bean=""/>
       ※local:本配置文件的bean
         bean: 除了本配置文件bean, 父容器引用也可以
       ~~~


6. Set注入简化写法

   - 基于属性简化

     ~~~markdown
     JDK类型注入
     <property name="name">
     	<value>wensu</value>
     </property>
     <property name="name" value="wensu" />
     ※ 通过value属性, 只能简化8种基本类型 + String注入标签
     
     用户自定义类型
     <property name="userDAO">
     	<ref bean="userDAO"/>
     </property>	
     <property name="userDAO" ref="userDAO"/>
     ~~~

   - 基于命名空间简化

     ~~~markdown
     JDK类型注入
     <bean id="person" class="***.Person">
         <property name="name">
             <value>wensu</value>
         </property>
     </bean>
     <bean id="person" class="***.Person" p:name="wensu"/>
     ※ 通过value属性, 只能简化8种基本类型 + String注入标签
     
     用户自定义类型
     <bean id="userService" class="***.UserServiceImpl">
         <property name="userDAO">
             <ref bean="userDAO"/>
         </property>
     </bean>
     <bean id="userService" class="***.UserServiceImpl" p:userDAO-ref="userDAO"/>
     ~~~

7. 构造注入

   ~~~markdown
   注入: 通过Spring配置文件, 为成员变量赋值
   Set注入: Spring调用Set方法, 通过配置文件, 为成员变量赋值
   构造注入: Spring调用构造方法, 通过配置文件, 为成员变量赋值
   ~~~

   - 开发步骤

     ~~~markdown
     1. 提供有参的构造方法
     2. Spring的配置文件
     ~~~

8. 注入总结

   ~~~markdown
   1 未来实战中, 应用set注入还是构造注入?
   2 answer: set 注入更多
              1. 构造注入麻烦(重载)
              2. Spring框架底层 大量应用了Set注入
   ~~~

   ![image-20230613070109075](https://github.com/wensugithub/markdownPicture/blob/main/image-20230613070109075.png?raw=true)

9. 反转控制与依赖注入

   - 反转控制(IOC Inverse Of Control)

     ~~~markdown
     控制: 对于成员变量赋值的控制权
           ※没有spring之前,通过代码完成对成员变量赋值。对成员变量赋值的控制权=代码 → 耦合
             Service中UserDao userDao = new UserDaoImpl()
           ※有spring之后, 对成员变量赋值的控制权=Spring配置文件 + Spring工厂 → 解耦合
           ⇒控制权发生了转移, 也说反转, 好处是解耦合
           底层实现: 工厂模式
     ~~~

     

     

   - sfsd

10. sdf

