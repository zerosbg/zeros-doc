# java相关面试题

### 基础：
- 1.Oracle JDK 和 OpenJDK 的对比
- 2.字符型常量和字符串常量的区别
- 3.构造器 Constructor 是否可被 override
- 4.String StringBuffer 和 StringBuilder 的区别是什么 String 为什么是不可变的
- 5.list 和 set的区别
- 6.HashSet 是如何保证不重复的
- 7.HashMap 是线程安全的吗，为什么不是线程安全的（最好画图说明多线程环境下不安全）?
- 8.HashMap 1.7 与 1.8 的 区别，说明 1.8 做了哪些优化，如何优化的？
- 9.Arrays.sort 和 Collections.sort 实现原理 和区别

### Spring相关
- BeanFactory 和 ApplicationContext 有什么区别
- Spring Bean 的生命周期
- Spring IOC 如何实现,什么是控制反转(IOC)？什么是依赖注入？
- Spring 框架中用到了哪些设计模式
- 动态代理（cglib 与 JDK）
- 什么是 Spring 框架？Spring 框架有哪些主要模块？
- 使用 Spring 框架能带来哪些好处？
- Spring Boot 有哪些优点？
- 什么是 JavaConfig？
- 如何重新加载 Spring Boot 上的更改，而无需重新启动服务器？
- 如何在 Spring Boot 中禁用 Actuator 端点安全性？
- 如何在自定义端口上运行 Spring Boot 应用程序?
- 使用 Spring Cloud 有什么优势？
- 负载平衡的意义什么？
- 什么是 Hystrix？它如何实现容错？
- 什么是 Netflix Feign？它的优点是什么？


### jvm
- 1.Java中是值传递还是引用传递？
- 2.构造器参数太多怎么办？
- 3.GC 收集器有哪些？CMS 收集器与 G1 收集器的特点。
- 4.Minor GC 与 Full GC 分别在什么时候发生？
- 5.简述 java 内存分配与回收策率以及 MinorGC 和Major GC

### 并发
- Synchronized 用 过 吗 ， 其原理 是 什 么 ？为 什 么 说 Synchronized 是 非 公 平 锁 ？
- 什 么 是 可 重 入 性 ， 为 什 么 说 Synchronized 是 可 重 入 锁 ？
- JVM 对 Java 的 原 生 锁 做 了 哪 些 优 化 ？
- 那 么 请 谈 谈 AQS 框 架 是 怎 么 回 事 儿 ？

### 多线程
- 多线程有什么用？
- Java 实现线程有哪几种方式？
- 线程中的 wait()和 sleep()方法有什么区别？
- 一个线程的生命周期有哪几种状态？它们之间如何流转的？
- 常用的几种线程池并讲讲其中的工作原理。

### MyBatis
- 讲下 MyBatis 的缓存
- 简述 Mybatis 的插件运行原理，以及如何编写一个插件？
- Mybatis 能执行一对一、一对多的关联查询吗？都有哪些实现方式，以及它们之间的区别？

### zookeeper
- 说下Zookeeper 集群管理（文件系统、通知机制）
- 四种类型的 znode 分别是那四种
- zookeeper 是如何保证事务的顺序一致性的？
- zookeeper 是如何选取主 leader 的？
