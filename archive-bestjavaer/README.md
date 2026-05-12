# bestJavaer 旧内容归档

这里是旧版 bestJavaer 的内容归档，主要包括 Java、操作系统、计算机网络、并发、JVM、MySQL、Spring、面试题和一些程序员相关内容。

![](https://www.cxuan.vip/image-20230415215250626.png)

![](http://img.shields.io/static/v1?label=bestjavaer&message=操作系统&color=blue)![](https://img.shields.io/static/v1?label=bestjavaer&message=计算机基础&color=<COLOR>)![](https://img.shields.io/static/v1?label=bestjavaer&message=计算机网络&color=yellowgreen)

![](http://img.shields.io/static/v1?label=bestjavaer&message=Java基础&color=orange)![](https://img.shields.io/static/v1?label=bestjavaer&message=设计模式&color=success)![](http://img.shields.io/static/v1?label=bestjavaer&message=JVM&color=important)![](https://img.shields.io/static/v1?label=bestjavaer&message=Java并发&color=9cf)

![](http://img.shields.io/static/v1?label=bestjavaer&message=Spring&color=blueviolet)![](http://img.shields.io/static/v1?label=bestjavaer&message=SpringBoot&color=informational)![](http://img.shields.io/static/v1?label=bestjavaer&message=Springcloud&color=ff69b4)

![](https://www.cxuan.vip/image-20230415215309372.png)

下面是这个仓库以前积累下来的 Java 和计算机基础内容：

* [Java基础面试题](https://github.com/crisxuan/bestJavaer/wiki/Java%E9%9D%A2%E8%AF%95%E9%A2%98)
* [操作系统](#%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%E7%B3%BB%E5%88%97)
* [计算机基础知识](#%E8%AE%A1%E7%AE%97%E6%9C%BA%E5%85%A5%E9%97%A8%E7%B3%BB%E5%88%97)
* [深入理解计算机系统](#%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%B3%BB%E7%BB%9F)
* [优质 github 推荐](#%E4%BC%98%E8%B4%A8-github-%E6%8E%A8%E8%8D%90)
* [HTTP 系列](#http-%E7%B3%BB%E5%88%97)
* [汇编语言](#%E6%B1%87%E7%BC%96%E8%AF%AD%E8%A8%80)
* [C 语言](#c-%E8%AF%AD%E8%A8%80)
* [计算机网络](#%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C%E7%B3%BB%E5%88%97)
* [Java 基础教程](#java-%E5%9F%BA%E7%A1%80%E7%B3%BB%E5%88%97)
* [设计模式](#%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E7%B3%BB%E5%88%97)
* [JVM](#jvm-%E7%B3%BB%E5%88%97)
* [并发](#%E5%B9%B6%E5%8F%91%E7%B3%BB%E5%88%97)
* [Spring 框架系列](#spring-%E7%B3%BB%E5%88%97)
  * [Spring](#spring-%E7%B3%BB%E5%88%97)
  * SpringMVC
  * [SpringBoot](#springboot-%E7%B3%BB%E5%88%97)
  * SpringCloud
  * SpringCloud-Alibaba
  * 等
* [ORM 映射框架](#mybatis)
  * [MyBatis](#mybatis)
  * JPA
  * Hibernate
* [ZooKeeper](#zookeeper-%E7%B3%BB%E5%88%97%E6%95%99%E7%A8%8B)
* [Kafka](#kafka-%E7%B3%BB%E5%88%97%E6%95%99%E7%A8%8B)
* [Redis](#redis-%E7%B3%BB%E5%88%97%E6%95%99%E7%A8%8B)
* [数据库](#mysql)
  * [MySQL](#mysql)
  * Oracle
  * MogonDB
  * PostgreSQL
  * Memcached
* RabbitMQ
* Maven
* Git
* Nginx
* ELK
* Netty
* [Linux](#linux-%E7%B3%BB%E5%88%97)
* [算法](#%E7%AE%97%E6%B3%95)
* 程序员
* [思维导图](#%E6%80%9D%E7%BB%B4%E5%AF%BC%E5%9B%BE)
* [关于认知](#%E5%85%B3%E4%BA%8E%E8%AE%A4%E7%9F%A5)
* [电子书籍](#%E7%94%B5%E5%AD%90%E4%B9%A6%E7%B1%8D)
* [我的PDF](#%E6%88%91%E7%9A%84-pdf)
* [读者系列](#%E8%AF%BB%E8%80%85%E9%9D%A2%E8%AF%95%E7%B3%BB%E5%88%97)
* [面试题系列](#%E9%9D%A2%E8%AF%95%E9%A2%98%E7%B3%BB%E5%88%97)
* [每日一题计划](#%E6%AF%8F%E6%97%A5%E4%B8%80%E9%A2%98%E8%AE%A1%E5%88%92)
* 书籍观后感

也包括一些常见的面试题。

采用全面解析面试题的方式，让你去理解每个面试题的概念，而不只是单纯的背诵...... 

不多说，搞起。

## 阅读须知

有一部分小伙伴/读者想让我出一个这个仓库的学习路线，否则不知道该从哪里开始看，我的建议是这样的（当然你可以不采纳）

计算机入门系列是小白必看的，这个系列会介绍 CPU、内存、磁盘、文件系统、操作系统的基础知识，这些知识都比较好理解，而且我也附了很多配图，通俗易懂。

其次是操作系统系列和计算机网络系列，这也是大学计算机科班的基础，想要在开发这个岗位走的更远，这些是很重要的方面。这两个系列是我觉得个人写的比较好的，而且系列内容我也在持续更新 ing ：）

HTTP 系列，HTTP 这个系列写的比较早，其中全面了解 HTTP 和 Cookie、Session 那篇获得很多小伙伴好评。

Linux 系列是能帮助你全面了解 Linux 操作系统的一个部分。

如果你是 Java 开发，可以看看本人写的 Java 基础系列和并发系列，这两个系列都是刨根问底形式的，我都研究到了字节码甚至 C++ 这一层。Spring 和 Mybatis 也可以看看（虽然我并没有写多少 逃...）

MySQL 这个部分比较基础，可以跟着一点一点写。

汇编语言和 C 语言还在更新 ing ......

其他的不想说了 ：）

## 操作系统系列👍

* [硬核操作系统入门](./operating-system/os-overview.md)
* [硬核操作系统之进程和线程](./operating-system/os-processandthread.md)
* [硬核操作系统之内存管理](./operating-system/os-rammanage.md)
* [硬核操作系统之文件系统](./operating-system/os-filesystem.md)
* [硬核操作系统之输入输出](./operating-system/os-inputoutput.md)
* [硬核操作系统之死锁](./operating-system/os-deadlock.md)
* [操作系统核心概念](./operating-system/os-importantconcept.md)
* [操作系统网站推荐](./operating-system/os-recommand.md)
* [操作系统超全面试题](./operating-system/os-interview-second.md)

## 计算机网络系列👍

* [计算机网络基础知识](./computer-network/computer-network-basic.md)
* [TCP/IP 基础知识](./computer-network/computer-network-tcpip.md)
* [计算机网络应用层](./computer-network/computer-application.md)
* [计算机网络传输层](./computer-network/computer-translayer.md)
* [计算机网络网络层](./computer-network/computer-internet.md)
* [计算机网络数据链路层](./computer-network/network-datalink.md)
* [一文了解 ARP 协议](./computer-network/network-arp.md)
* [一文了解 DNS 协议](./computer-network/network-dns.md)
* [一文了解 ICMP 协议](./computer-network/network-icmp.md)
* [一文了解 DHCP 协议](./computer-network/network-dhcp.md)
* [一文了解 NAT 协议](./computer-network/network-nat.md)
* [Web 页面的请求流程，超详细](./computer-network/web-request.md)
* [什么是 Socket](./computer-network/network-socket.md)
* [一文了解路由选择协议](./computer-network/network-routerchoose.md)
* [一文了解 HTTP/2.0](./computer-network/network-http2.0.md)
* [一文了解 QUIC 协议](./computer-network/network-quic.md)
* [一文了解 HTTP/3.0](./computer-network/network-http3.0.md)
* [计算机网络自学指南](./computer-network/computer-howtolearn.md)
* [计算机网络核心概念](./computer-network/network-concepts.md)
* [计算机网络发展史](./computer-network/network-history.md)
* [学计算机网络，看计算机自顶向下好还是谢希仁的计算机好](./computer-network/network-choose.md)

## Java 基础系列👍

* [Java 基础核心总结](./java-basic/java-summary.md)
* [Java 代理](./java-basic/java-proxy.md)
* [Java 反射](./java-basic/java-reflect.md)
* [Java 集合](./java-basic/java-collections.md)
* [String、StringBuffer 和 StringBuilder](./java-basic/java-stringstringbufferstringbuilder.md)
* [Java 中的语法糖](./java-basic/java-suger.md)
* [深入理解 static 关键字](./java-basic/java-static.md)
* [深入理解 Java 变量](./java-basic/java-varaibles.md)
* [深入理解 final、finally、finalize](./java-basic/java-final.md)
* [浅拷贝和深拷贝](./java-basic/java-clone.md)
* [关于四种引用类型](./java-basic/java-references.md)
* [Java 开发最容易忽视的 10 个 Bug](./java-basic/java-ignoretenmistakes.md)
* [Java 浅拷贝和深拷贝](./java-basic/java-clone.md)
* [Java 创建对象的五种方式](./java-basic/java-createobject.md)
* [Exception 和 Error 的区别](./java-basic/java-exceptionanderror.md)
* [for 、foreach 、iterator 三种遍历方式的比较](./java-basic/java-forandforeach.md)
* [理解静态绑定与动态绑定](./java-basic/java-staticbinding.md)
* [@SuppressWarnings 用法](./java-basic/java-%40suppresswarnings.md)
* Arrays.asList 解析
* [Comparable 和 Comparator的理解](./java-basic/java-comparableandcomparator.md)
* [学习 Java 网站推荐给你](./java-basic/learn-java.md)

### Java 基础源码分析

[看完这篇 HashMap，和面试官扯皮就没问题了](./java-basic/java-hashmap.md)

## 并发系列

* [JSR-133 都解决了哪些问题](./java-concurrent/java-jsr133.md)
* [简单认识并发](./java-concurrent/java-concurrent-basic.md)
* [看完你就明白的锁系列之锁的状态](./java-concurrent/java-lock-status.md)
* [看完你就明白的锁系列之乐观锁和悲观锁](./java-concurrent/java-optimisticlock.md)
* [看完你就明白的锁系列之自旋锁](./java-concurrent/java-spinlock.md)
* [锁系列汇总](./java-concurrent/java-lock.md)
* [并发编程超强入门汇总](./java-concurrent/java-concurrent.md)

### 并发源码分析

* [ReentrantLock 源码分析](./java-concurrent/java-reentrantlock.md)
* [LongAddr 用法和源码分析](./java-concurrent/java-longaddr.md)
* [JSR - 133 都有哪些内容](./java-concurrent/java-jsr133.md)
* [我花了 35 张图就为你让你了解 AQS](./java-concurrent/java-aqs.md)
* [AtomicInteger 的用法和实现原理](./java-concurrent/java-atomicInteger.md)
* [CountDownLatch 用法和源码解释](./java-concurrent/java-countDownLatch.md)
* [Atomic 基本数据类型的用法和实现原理](./java-concurrent/java-atomicxxx.md)
* [AtomicReference 的用法和源码解析](./java-concurrent/java-atomicReference.md)
* [线程池超用心源码分析](./java-concurrent/java-threadpoolexecutor.md)
* [深入理解 volatile 关键字](./java-concurrent/java-volatile.md)
* [Semaphore 用法和源码分析](./java-concurrent/java-semaphore.md)
* [happens - before 原则剖析](./java-concurrent/java-happensbefore.md)

## 计算机入门系列

* [程序员需要了解的硬核知识之 CPU](./computer-basic/computer-cpu.md)
* [程序员需要了解的硬核知识之内存](./computer-basic/computer-ram.md)
* [程序员需要了解的硬核知识之二进制](./computer-basic/computer-binary.md)
* [程序员需要了解的硬核知识之磁盘](./computer-basic/computer-disk.md)
* [程序员需要了解的硬核知识之压缩算法](./computer-basic/computer-compression.md)
* [程序员需要了解的硬核知识之操作系统和应用](./computer-basic/computer-osandapp.md)
* [程序员需要了解的硬核知识之操作系统入门](./computer-basic/computer-os.md)
* [程序员需要了解的硬核知识之控制硬件](./computer-basic/computer-hardware.md)

## 深入理解计算机系统

* [计算机系统入门概述](./computersystem/csapp-basic.md)

## HTTP 系列

* [全面了解 HTTP](./http/http-basic.md)
* [HTTP 黑科技](./http/http-advanced.md)
* [HTTP 核心概念](./http/http-deepknow.md)
* [全面了解 HTTPS](./http/http-https.md)
* [全面了解 Cookies、Session 和 Token](./http/http-cookesessiontoken.md)
* [图解 HTTP 连接管理](./http/http-manageconnection.md)

## Linux 系列

* [Linux 操作系统之开篇！！！](./linux/linux-first.md)
* [Linux 操作系统之进程和线程](./linux/linux-processandthread.md)
* [Linux 操作系统之内存管理](./linux/linux-memroy-management.md)
* [Linux 操作系统之IO管理](./linux/linux-io.md)
* [Linux 操作系统之文件系统](./linux/linux-file-system.md)
* [Linux 硬件和 BIOS](./linux/linux-hardware.md)
* [Linux 中断机制](./linux/linux-interrupt.md)
* [Linux 分段机制](./linux/linux-segmentation.md)
* [Linux 分页机制](./linux/linux-pagination.md)
* [Linux 保护机制](./linux/linux-protect.md)
* [为什么 x86 中 BIOS 会把 MBR 放在 0x7c00 处](./linux/whyx86loadsMBR.md)


## 设计模式系列

* [设计模式基础入门](./design-pattern/designpattern-basic.md)
* [我向面试官讲解了单例模式，他对我竖起了大拇指](./design-pattern/designpattern-singlaton.md)

## JVM 系列

* [JVM 面试题总结](./jvm/jvm-interviewanswer.md)

## 汇编语言

* [从指令集的角度看汇编](./assembly/assembly01-basic.md)
* [寄存器入门第一篇](./assembly/assembly02-register.md)
* [汇编 Debug 实战](./assembly/assembly03-debug.md)
* [x86 汇编循环指令](./assembly/assembly04-loop.md)
* [x86 汇编 debug 循环指令](./assembly/assembly05-debugprogram.md)
* [简单了解下段](./assembly/assembly06-segment.md)
* [多个段的程序](./assembly/assembly07-moresegment.md)

## C 语言

* [C 语言基础入门](./cprograming/c-basic.md)
* [C 语言数据](./cprograming/c-data.md)
* [C 函数与程序控制](./cprograming/c-function.md)

## MyBatis

* [MyBatis 基础搭建及架构概述](./mybatis/mybatis-base.md)
* [MyBatis Configuration](./mybatis/mybatis-configuration.md) 
* [MyBatis 核心配置综述之Executor](./mybatis/mybatis-executor.md)
* [MyBatis 核心配置综述之 StatementHandler](./mybatis/mybatis-statmenthandler.md)
* [MyBatis 核心配置综述之 ParameterHandler](./mybatis/mybatis-parameterhandler.md)
* [MyBatis 核心配置综述之 ResultSetHandler](./mybatis/mybatis-resultsethandler.md)
* [MyBatis 一级缓存](./mybatis/mybatis-firstcache.md)
* [MyBatis 二级缓存全详解](./mybatis/mybatis-secondcache.md)
* [MyBatis 启动流程](./mybatis/mybatis-howtostart.md)

## MySQL

* [MySQL 基础入门大全](./mysql/mysql-basicall.md)
* [MySQL 开发](./mysql/mysql-develop.md)
* [MySQL 进阶技巧](./mysql/mysql-improve.md)
* [MySQL 调优](./mysql/mysql-optimization.md)

## Spring 系列

* [Spring Bean 全解析](./spring/spring-bean.md)
* [Spring AOP 扫盲](./spring/spring-aop.md)
* [Spring 注解配置的基本要素](./spring/spring-annotation.md)
* [Spring 中的 Null-Safety](./spring/spring-null-safety.md)
* [Spring 中的验证、数据绑定和类型转换](./spring/spring-databind.md)
* [PropertyPlaceholderConfigurer 用法](./spring/spring-propertyplaceholderconfig.md)
* [BeanFactory 和 FactoryBean 的理解](./spring/spring-beanfactoryandfactorybean.md)
* [BeanFactory 和 ApplicationContext 的异同](./spring/spring-beanandapplication.md)
* [浅析PropertySource 基本使用](./spring/spring-propertysource.md)
* [一文了解ConfigurationConditon 接口](./spring/spring-configurationcondition.md)
* [@Configuration 全部用法](./spring/spring-configuration.md)
* [Spring Resource 体系介绍](./spring/spring-resource.md)

## Kafka 系列教程

* [真的，Kafka 入门一篇就够了](./kafka/kafka-basic.md)
* [你能说出这些 Kafka 的原理吗](./kafka/kafka-deep.md)

## ZooKeeper 系列教程

* [ZooKeeper 基础入门](./zookeeper/zookeeper-basic.md)

## 读者系列

* [今年面试这么难，到底如何进入大厂？](./reader/interview-jingdong.md)
* [外包面试之旅](./reader/interview-zhongruan.md)
* [京东面试之旅](./reader/interview-jingdong-social.md)
* [百度面试之旅](./reader/interview-baidu.md)
* [读者考研之旅](./reader/interview-kaoyan.md)
* [上海妹子的一天是怎样的](./reader/thedayofshanghai.md)

## 面试题系列

> 笔者非常痛恨网上那种什么面试题汇总等文章，无非就是各种百度拿了前几句滥竽充数一样，这种宣扬背诵的做法和高中老师教学生应付考试是一样的，侥幸心理、凡事图快的心理才助长了社会浮躁的风气。
>
> 所以笔者励志把每道面试题从根源上助你理解

* [HTTP 高频面试题](./interview-answer/http-interview.md)
* [用心为你写了 9 道 MySQL 面试题](./interview-answer/mysql-interview.md)
* [Java 基础面试题汇总](./interview-answer/java-basic-interview.md)

* [操作系统面试题](./operating-system/os-fiftyInterview.md)

## 优质 github 推荐

* [JavaGuide 的 github](https://github.com/Snailclimb/JavaGuide) 

* [小傅哥的 Github](https://github.com/fuzhengwei/CodeGuide/wiki)

## 思维导图

* [更好的Java程序员](./mindmanage/bestjavaer.png)
* [设计模式](./mindmanage/design-pattern.png)
* [Java并发](./mindmanage/java-concurrent.png)
* [JVM](./mindmanage/jvm.png)
* [Kafka体系](./mindmanage/kafka-system.png)
* [MyBatis体系](./mindmanage/mybatis.png)
* [MySQL](./mindmanage/mysql.png)
* [Nginx](./mindmanage/nginx.png)
* [Redis](./mindmanage/redis.png)
* [Spring](./mindmanage/spring.png)
* [ZooKeeper](./mindmanage/zookeeper.png)
* [程序员必备硬核知识](./mindmanage/computer-basic.png)
* [现代操作系统](./mindmanage/operating-system.png)
* [Java 基础核心总结](./mindmanage/java-basic.png)
* [HTTP 核心总结](./mindmanage/http.png)
* [Java.lang 包](./mindmanage/java-lang.png)
* [I/O 流](./mindmanage/java-io.png)
* [Session、Cookie 和 Token](./mindmanage/sessioncookieandtoken.png)
* [锁的分类](./mindmanage/java-lock.png)
* [AQS 框架](./mindmanage/java-aqs.png)
* [Java.net 包](./mindmanage/java-net.png)

## 关于 cxuan

* [2019 我是怎样熬过来的](./aboutlife/cxuan-2019.md)
* [这是对我最大的认可和鼓励](./aboutlife/cxuan-confidence.md)
* [1w+ 的心路历程](./aboutlife/cxuan-1w%2B.md)
* [美国留学生关于教育、制度和考试的看法](./aboutlife/americdan-life.md)
* [内心独白｜给粉蜜的一封信](./aboutlife/cxuan-say.md)
* 给朋友们一些自信｜写于2019年4月
* [作者的一周](./aboutlife/cxuan-oneweek.md)
* [bilibili 关于后浪有感](./aboutlife/aboutbilibili.md)
* [电信诈骗](./aboutlife/cxuan-deceive.md)
* [如何成为务实的程序员](./studysuggestion/good-programmer.md)
* [写给 25 岁的自己](./aboutlife/cxuan-25yearsold.md)
* [面试官和面试者在同一个群里是怎样的体验](./aboutlife/interviewer-story.md)
* [程序员的水平能有多低？](./aboutlife/cxuan-lowprogrammer.md)
* [自媒体技术的困境](./aboutlife/selfmedia-difficult.md)
* [cxuan 结婚啦！！！](./aboutlife/cxuan-marrige.md)
* [cxuan 国庆的躺平经历](./aboutlife/cxuan-nationalday.md)
* [陪媳妇暑假去旅游了](./aboutlife/cxuan-trip.md)

## ChatGPT

* [我的 ChatGPT 梭哈操作系统](./chatgpt/chatGPT-operating-system.md)

## 程序员

* [程序员都必知的一些网站](./programmer/website-recommand.md)
* [世界上最伟大的女程序员](./programmer/thegreatffemaleprogrammerintheworld.md)
* [我眼里的左耳朵耗子](./programmer/never-say-goodbay-collshell.md)



## 每日一题计划

* [2020 年每日一题](./reader/2020-interview-everyday.md)

## 书籍观后感

* [如何评价《Java 并发编程艺术》这本书？](./book-view/read-theArtOfJavaConcurrencyProgramming.md)
* [如何评价《On Java 中文版》这本书？](./book-view/read-onJava.md)
* [《CSAPP》是一本什么书？](./book-view/read-csapp.md)


## 欢迎关注

欢迎关注作者的微信公众号「cxuanAI」，cxuanAI 目前已经全部接入 OpenClaw ，后台会有一个甜心的小妹妹来和你对话。

![](https://z3.ax1x.com/2020/12/11/rkf8A0.jpg)


