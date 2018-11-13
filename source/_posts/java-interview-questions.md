---
title: Java面试题
date: 2018-11-10 09:50:33
tags: [java]
---

1. 面向对象的特征有哪些方面？
2. 访问修饰符`public`、`private`、`protected`以及不写（默认）时的区别？
3. `String`是最基本的数据类型吗？
4. `float f=3.4;`是否正确？
5. `short s1 = 1; s1 = s1 + 1;`有错吗?`short s1 = 1; s1 += 1;`有错吗？
6. Java有没有`goto`？
7. `int`和`Integer`有什么区别？
8. `&`和`&&`的区别？
9.  解释内存中的栈(stack)、堆(heap)和静态区(static area)的用法。
10. `Math.round(11.5)` 等于多少？`Math.round(-11.5)`等于多少？
11. `switch`是否能作用在`byte`上，是否能作用在`long`上，是否能作用在`String`上？
12. 用最有效率的方法计算2乘以8？
13. 数组有没有`length()`方法？`String`有没有`length()`方法？
14. 在Java中，如何跳出当前的多重嵌套循环？
15. 构造器（constructor）是否可被重写（override）？
16. 两个对象值相同(`x.equals(y) == true`)，但却可有不同的hash code，这句话对不对？
17. 是否可以继承`String`类？
18. 当一个对象被当作参数传递到一个方法后，此方法可改变这个对象的属性，并可返回变化后的结果，那么这里到底是值传递还是引用传递？
19. `String`和`StringBuilder`、`StringBuffer`的区别？
20. 重载（Overload）和重写（Override）的区别。重载的方法能否根据返回类型进行区分？<!--more-->
21. 描述一下JVM加载class文件的原理机制？
22. `char`型变量中能不能存贮一个中文汉字，为什么？
23. 抽象类（abstract class）和接口（interface）有什么异同？
24. 静态嵌套类(Static Nested Class)和内部类（Inner Class）的不同？
25. Java中会存在内存泄漏吗，请简单描述。
26. 抽象的（abstract）方法是否可同时是静态的（static）,是否可同时是本地方法（native），是否可同时被`synchronized`修饰？
27. 阐述静态变量和实例变量的区别。
28. 是否可以从一个静态（static）方法内部发出对非静态（non-static）方法的调用？
29. 如何实现对象克隆？
30. `String s = new String("xyz");`创建了几个字符串对象？
31. 接口是否可继承（extends）接口？抽象类是否可实现（implements）接口？抽象类是否可继承具体类（concrete class）？
32. 一个`.java`源文件中是否可以包含多个类（不是内部类）？有什么限制？
33. Anonymous Inner Class(匿名内部类)是否可以继承其它类？是否可以实现接口？
34. 内部类可以引用它的包含类（外部类）的成员吗？有没有什么限制？
35. Java中的`final`关键字有哪些用法？
36. 数据类型之间的转换：
37. 如何实现字符串的反转及替换？
38. 怎样将`GB2312`编码的字符串转换为`ISO-8859-1`编码的字符串？
39. 日期和时间：
40. 打印昨天的当前时刻。
41. 比较一下Java和JavaSciprt。
42. 什么时候用断言（assert）？
43. `Error`和`Exception`有什么区别？
44. `try{}`里有一个`return`语句，那么紧跟在这个`try`后的`finally{}`里的代码会不会被执行，什么时候被执行，在`return`前还是后?
45. Java语言如何进行异常处理，关键字：`throws`、`throw`、`try`、`catch`、`finally`分别如何使用？
46. 运行时异常与受检异常有何异同？
47. 列出一些你常见的运行时异常？
48. 阐述`final`、`finally`、`finalize`的区别。
49. `List`、`Set`、`Map`是否继承自`Collection`接口？
50. 阐述`ArrayList`、`Vector`、`LinkedList`的存储性能和特性。
51. `Collection`和`Collections`的区别？
52. `List`、`Map`、`Set`三个接口存取元素时，各有什么特点？
53. `TreeMap`和`TreeSet`在排序时如何比较元素？`Collections`工具类中的`sort()`方法如何比较元素？
54. `Thread`类的`sleep()`方法和对象的`wait()`方法都可以让线程暂停执行，它们有什么区别?
55. 线程的`sleep()`方法和`yield()`方法有什么区别？
56. 当一个线程进入一个对象的`synchronized`方法A之后，其它线程是否可进入此对象的`synchronized`方法B？
57. 请说出与线程同步以及线程调度相关的方法。
58. 编写多线程程序有几种实现方式？
59. `synchronized`关键字的用法？
60. 举例说明同步和异步。
61. 启动一个线程是调用`run()`还是`start()`方法？
62. 什么是线程池（thread pool）？
63. 线程的基本状态以及状态之间的关系？
64. 简述`synchronized`和`java.util.concurrent.locks.Lock`的异同？
65. Java中如何实现序列化，有什么意义？
66. Java中有几种类型的流？
67. 写一个方法，输入一个文件名和一个字符串，统计这个字符串在这个文件中出现的次数。
68. 如何用Java代码列出一个目录下所有的文件？
69. 用Java的套接字编程实现一个多线程的回显（echo）服务器。
70. XML文档定义有几种形式？它们之间有何本质区别？解析XML文档有哪几种方式？
71. 你在项目中哪些地方用到了XML？
72. 阐述JDBC操作数据库的步骤。
73. `Statement`和`PreparedStatement`有什么区别？哪个性能更好？
74. 使用JDBC操作数据库时，如何提升读取数据的性能？如何提升更新数据的性能？
75. 在进行数据库编程时，连接池有什么作用？
76. 什么是DAO模式？
77. 事务的ACID是指什么？
78. JDBC中如何进行事务处理？
79. JDBC能否处理Blob和Clob？
80. 简述正则表达式及其用途。
81. Java中是如何支持正则表达式操作的？
82. 获得一个类的类对象有哪些方式？
83. 如何通过反射创建对象？
84. 如何通过反射获取和设置对象私有字段的值？
85. 如何通过反射调用对象的方法？
86. 简述一下面向对象的"六原则一法则"。
87. 简述一下你了解的设计模式。
88. 用Java写一个单例类。
89. 什么是UML？
90. UML中有哪些常用的图？
91. 用Java写一个冒泡排序。
92. 用Java写一个折半查找。
93. 阐述Servlet和CGI的区别?
94. Servlet接口中有哪些方法？
95. 转发（forward）和重定向（redirect）的区别？
96. JSP有哪些内置对象？作用分别是什么？
97. get和post请求的区别？
98. 常用的Web服务器有哪些？
99. JSP和Servlet是什么关系？
100. 讲解JSP中的四种作用域。
101. 如何实现JSP或Servlet的单线程模式？
102. 实现会话跟踪的技术有哪些？
103. 过滤器有哪些作用和用法？
104. 监听器有哪些作用和用法？
105. web.xml文件中可以配置哪些内容？
106. 你的项目中使用过哪些JSTL标签？
107. 使用标签库有什么好处？如何自定义JSP标签？
108. 说一下表达式语言（EL）的隐式对象及其作用。
109. 表达式语言（EL）支持哪些运算符？
110. Servlet3中的异步处理指的是什么？
111. 如何在基于Java的Web项目中实现文件上传和下载？
112. 服务器收到用户提交的表单数据，到底是调用Servlet的`doGet()`还是`doPost()`方法？
113. JSP中的静态包含和动态包含有什么区别？
114. Servlet中如何获取用户提交的查询参数或表单数据？
115. Servlet中如何获取用户配置的初始化参数以及服务器上下文参数？
116. 如何设置请求的编码以及响应内容的类型？
117. 解释一下网络应用的模式及其特点。
118. 什么是Web Service（Web服务）？
119. 概念解释：SOAP、WSDL、UDDI。
120. Java规范中和Web Service相关的规范有哪些？
121. 介绍一下你了解的Java领域的Web Service框架。
122. 什么是ORM？
123. 持久层设计要考虑的问题有哪些？你用过的持久层框架有哪些？
124. Hibernate中`SessionFactory`是线程安全的吗？`Session`是线程安全的吗（两个线程能够共享同一个`Session`吗）？
125. Hibernate中`Session`的`load`和`get`方法的区别是什么？
126. `Session`的`save()`、`update()`、`merge()`、`lock()`、`saveOrUpdate()`和`persist()`方法分别是做什么的？有什么区别？
127. 阐述`Session`加载实体对象的过程。
128. `Query`接口的`list`方法和`iterate`方法有什么区别？
129. Hibernate如何实现分页查询？
130. 锁机制有什么用？简述Hibernate的悲观锁和乐观锁机制。
131. 阐述实体对象的三种状态以及转换关系。
132. 如何理解Hibernate的延迟加载机制？在实际应用中，延迟加载与`Session`关闭的矛盾是如何处理的？
133. 举一个多对多关联的例子，并说明如何实现多对多关联映射。
134. 谈一下你对继承映射的理解。
135. 简述Hibernate常见优化策略。
136. 谈一谈Hibernate的一级缓存、二级缓存和查询缓存。
137. Hibernate中`DetachedCriteria`类是做什么的？
138. `@OneToMany`注解的`mappedBy`属性有什么作用？
139. MyBatis中使用`#`和`$`书写占位符有什么区别？
140. 解释一下MyBatis中命名空间（namespace）的作用。
141. MyBatis中的动态SQL是什么意思？
142. 什么是IoC和DI？DI是如何实现的？
143. Spring中Bean的作用域有哪些？
144. 解释一下什么叫AOP（面向切面编程）？
145. 你是如何理解"横切关注"这个概念的？
146. 你如何理解AOP中的连接点（Joinpoint）、切点（Pointcut）、增强（Advice）、引介（Introduction）、织入（Weaving）、切面（Aspect）这些概念？
147. Spring中自动装配的方式有哪些？
148. Spring中如何使用注解来配置Bean？有哪些相关的注解？
149. Spring支持的事务管理类型有哪些？你在项目中使用哪种方式？
150. 如何在Web项目中配置Spring的IoC容器？
151. 如何在Web项目中配置Spring MVC？
152. Spring MVC的工作原理是怎样的？
153. 如何在Spring IoC容器中配置数据源？
154. 如何配置配置事务增强？
155. 选择使用Spring框架的原因（Spring框架为企业级开发带来的好处有哪些）？
156. Spring IoC容器配置Bean的方式？
157. 阐述Spring框架中Bean的生命周期？
158. 依赖注入时如何注入集合属性？
159. Spring中的自动装配有哪些限制？
160. 在Web项目中如何获得Spring的IoC容器？
161. 大型网站在架构上应当考虑哪些问题？
162. 你用过的网站前端优化的技术有哪些？
163. 你使用过的应用服务器优化技术有哪些？
164. 什么是XSS？什么是SQL注入？什么是CSRF？
165. 什么是领域模型(domain model)？贫血模型(anaemic domain model)和充血模型(rich domain model)有什么区别？
166. 谈一谈测试驱动开发（TDD）的好处以及你的理解。
