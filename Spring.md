* Spring环境搭建

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
  
* 注意点

  1. Spring工厂是可以调用对象的私有构造方法来创建对象的
  2. 实体对象(Entity)会封装数据库中表的数据, 不是由Spring工厂来创建,  而是交给持久层框架来创建(Mybatis,Hibernate,JPA)

* Spring5.x与日志框架的整合

  1. Spring与日志框架整合, 日志框架就可以在控制台中, 输出Spring运行过程中的重要信息。

  2. 好处: 便于了解Spring框架的运行过程, 利于程序的调试。

  3. Spring如何整合日志框架

     ~~~markdown
     
     ~~~

     