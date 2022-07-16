# 0. 常识

## 1. 什么是Spring框架？

Spring框架包含众多模块。这些模块的功能都是建立在IoC和AOP之上的，所以IoC和AOP是Spring框架的核心。Spring 最核心的思想就是不重新造轮子，开箱即用！

> - IoC（Inversion of Control）是控制反转的意思。
>
>   - 作用是降低对象间的耦合性。
>
>   - 例子：租房子。把你需要的房型告诉中介，就可以选到需要的房子，中介就是Spring容器。
>
> - AOP（Aspect Oriented Programing）是面向切面编程思想。是对面向对象（OOP）开发的补充。
>
>   - 作用是降低业务逻辑之间的耦合性。
>   - 不改变源码，增强原有功能。



## 2. （重要）Bean 的生命周期

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220413165759229.png" alt="image-20220413165759229" style="zoom: 50%;" />





# 1. Spring



## （1）IOC

**为什么叫控制反转？** 原先OOP中，对象的创建和依赖关系都由程序自己控制。

**IOC 思想就是将 创建对象的控制权交给Spring框架。将对象之间的相互依赖关系交给 IoC 容器来管理，并由 IoC 容器完成对象的注入。**

> - IoC 容器：是 Spring 用来实现 IoC 的载体， IoC 容器实际上就是个 Map（key，value），Map 中存放的是各种对象。
>
> - 依赖注入(Dependency Injection)：DI 是 IOC 的具体实现。Spring 将持久层的对象传入到了业务层，只需要获取业务层的实例就可以调用持久层的方法。 即实现 DI 后，就可以不用去获取持久层对象了，只需获取业务层对象。
>
>   <img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220318154517058.png" alt="image-20220318154517058" style="zoom:67%;" />

- **控制** ：指的是对象创建（实例化、管理）的权力
- **反转** ：控制权交给外部环境（Spring 框架、IoC 容器）

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220324095957161.png" alt="image-20220324095957161" style="zoom:50%;" />



### IOC 的注入方式

**DI（Dependency Injection），它是IoC实现的实现方式，就是说IoC是通过DI来实现的。**



- Set 方法注入（常用）

> 要求 业务层 必须有 持久层的 Set 方法。

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220318154921144.png" alt="image-20220318154921144" style="zoom: 80%;" />



- 构造方法注入

  <img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220318155321206.png" alt="image-20220318155321206" style="zoom:80%;" />





#### （1）依赖注入的相关注解？

spring可以自动帮你把bean里面引用的对象的setter/getter方法省略，它会自动帮你set/get。

- @Autowired ：**自动按类型注⼊**，如果有多个匹配 则按照指定 Bean 的 id 查找，查找不到会报错。

  > 给方法注⼊时可单独使⽤。直接按照 Bean 的 id 注入，只能注入Bean 类型。

- @Qualifier ：如果容器中有一个以上匹配的Bean，则可以通过@Qualifier注解**指定注入Bean的名称**

如：当一个接口实现有两个实现类时，只使用@Autowired注解，会报错。原因是存在两个实例，系统不知道要注入哪个实例，所以就需要用到@Qualifier注解来指明注入的实例。



## （2）AOP 

AOP(Aspect-Oriented Programming:面向切面编程)，降低业务之间的耦合性。

> 名词解释：
>
> - 连接点：可以被增强的方法
> - 切入点：实际被增强的方法
> - 通知（增强）：即  对原有方法的增强
> - 切面（织入）：把 通知 应用到 切入点的 动作， 切面 = 切入点 + 通知



### 1. AOP 底层使用动态代理

> **一般使用 AspectJ 框架 与 Spring 框架一起实现 AOP**

有两种情况动态代理 : **看目标类有没有实现接口**。

- **JDK动态代理** ：有接口
- **CGLIB** **动态代理**：没有接口





### 2. Spring Bean

> - **bean 代指的就是那些被 IoC 容器所管理的 java对象。**我们需要告诉 IoC 容器帮助我们管理哪些对象
>
> - **Spring通过IoC容器来管理Bean，我们可以通过XML配置或者注解配置，来指导IoC容器对Bean的管理。**

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220321104711536.png" alt="image-20220321104711536" style="zoom:50%;" />

#### (1) Bean 的作用域有哪些？

> 可以通过@Scope注解修改Bean的作用域

Spring 中 Bean 的作用域通常有下面几种：

- **singleton** : 单例模式， **唯一 bean 实例**，Spring 中的 bean 默认都是单例的，对单例设计模式的应用。
  - 生命周期 ： 应用加载后，Bean就被创建。应用结束时，销毁Bean。
- **prototype** : 多例模式， 每次访问对象，都会创建一个新的 bean 实例。
  - 生命周期 ：使用对象时，创建新的Bean。长时间不使用，Bean 就被回收。
- **request** : 每一次 HTTP 请求都会产生一个新的 bean，该 bean 仅在当前 Spring Applicationcontext 内有效。
  - 生命周期 ：请求处理结束后，销毁Bean。SpringMVC 每次request 就会产生一个新的bean。
- **session** : 每一次来自新 session 的 HTTP 请求都会产生一个新的 bean，该 bean 仅在当前 HTTP session 内有效。
- **global-session** ： 全局 session 作用域，仅仅在基于 portlet 的 web 应用中才有意义，Spring5 已经没有了。Portlet 是能够生成语义代码(例如：HTML)片段的小型 Java Web 插件。它们基于 portlet 容器，可以像 servlet 一样处理 HTTP 请求。但是，与 servlet 不同，每个 portlet 都有不同的会话。



#### （2）@Component 和 @Bean 的区别是什么？

- `@Component`:
  - **使用在 类 上，用于将 类 实例化到spring容器中**。即 **将该类注册到 Ioc容器 中，成为Spring Bean**
  - 相当于配置文件中的 <bean  class="全类名"/>

- `@Bean`:
  - **使用在 方法 上，表示将该方法的返回值存储到Spring容器中。**
  - 相当于配置文件中的 <bean  id="方法名"/>





#### （3）将一个类声明为 bean 的注解有哪些?

我们**一般使用 `@Autowired` 注解自动装配 bean**，要想把类标识成可用于 `@Autowired` 注解自动装配的 bean 的类,采用以下注解可实现：

> **@Autowired的作用：spring可以自动帮你把bean里面引用的对象的setter/getter方法省略，它会自动帮你set/get。**

- `@Component` ：**通用的注解，用于将 类 实例化到spring容器中。如果一个 Bean 不知道属于哪个层，可以使用`@Component` 注解标注。**
- `@Repository` : 对应持久层即 Dao 层，主要用于数据库相关操作。
- `@Service` : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao 层。
- `@Controller` : 对应 Spring MVC 控制层，主要用户接受用户请求并调用 Service 层返回数据给前端页面。



#### （4）Bean 的生命周期

Spring容器管理Bean，涉及对Bean的创建、初始化、调用、销毁等一系列的流程，这个流程就是Bean的生命周期。整个流程参考下图：

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220324101155928.png" alt="image-20220324101155928" style="zoom:50%;" />

**这个过程是由Spring容器自动管理的**，其中有两个环节我们可以进行干预。

1. 我们**可以自定义初始化方法**，并在该方法前增加@PostConstruct注解，届时Spring容器将在调用SetBeanFactory方法之后调用该方法。
2. 我们**可以自定义销毁方法**，并在该方法前增加@PreDestroy注解，届时Spring容器将在自身销毁前，调用这个方法。





## （3）BeanFactory 和ApplicationContext 的区别？

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220324101331777.png" alt="image-20220324101331777" style="zoom:67%;" />

- **BeanFactory :**  

  - **是IOC容器的底层实现接口**，**可以理解为含有Bean 集合的工厂类**, 是ApplicationContext的顶级接口；
  - **Spring不允许我们直接操作BeanFactory，所以为我们提供了ApplicationContext这个接口**；

- **ApplicationContext:**

  > 在使用Spring的时候，我们经常需要**先得到一个ApplicationContext对象，然后从该对象中获取我们配置的Bean对象。**
  >
  > - 如：<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220324102131690.png" alt="image-20220324102131690" style="zoom:50%;" />

  - 是 BeanFactory 的扩展子接口
  - **作用：可以通过 ApplicationContext 接口获得 Spring容器的Bean 对象**
  - 常用实现类：
    - <img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220321114403459.png" alt="image-20220321114403459" style="zoom: 33%;" />





## （4）@Autowired和@Resource注解有什么区别？

- @Autowried :  放在 属性 或者 set 方法上，既不用再写set方法了
  - 是Spring提供的注解
  - **只能按类型注入**
  - 如果容器中有一个以上匹配的Bean，则可以通过@Qualifier注解**指定注入Bean的名称**
  - 如果允许null值，可以设置它required属性为false。 @Autowried(required = false)



- @Resource  :   放在属性上，**@ Resource= @Autowired   +   @Qualifier**
  - **是JDK提供的注解**
  - **@Resource默认按名称注入**，也支持按类型注入
  - @Resource 先按名称注入，若失败，则按类型注入







## （5）Spring如何管理事务？

事务添加到 JavaEE 三层结构里面Service层（业务逻辑层）。

- **在** **Spring** **进行事务管理操作**

  - 有两种方式：编程式事务管理和**声明式事务管理（使用）**

  - 声明式事务管理

    - **基于注解方式（使用）**

      > **在service 类上添加 @Transactional**

    - XML配置文件





## （6）Spring 框架中用到了哪些设计模式？

- **工厂设计模式** : Spring 使用工厂模式通过 `BeanFactory`、`ApplicationContext` 创建 bean 对象。
- **代理设计模式** : Spring AOP 功能的实现。
- **单例设计模式** : Spring 中的 Bean 默认都是单例的。
- **模板方法模式** : Spring 中 `jdbcTemplate`等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。







# 2. SpringMVC

>  MVC是一种设计模式，在这种模式下软件被分为三层，即Model（模型）、View（视图）、Controller（控制器）。Model代表的是数据，View代表的是用户界面，Controller代表的是数据的处理逻辑，它是Model和View这两层的桥梁。将软件分层的好处是，可以将对象之间的耦合度降低，便于代码的维护。

Spring MVC 是一款很优秀的 MVC 框架。Spring MVC 下我们一般把后端项目分为 Service 层（处理业务）、Dao 层（数据库操作）、Entity 层（实体类）、Controller 层(控制层，返回数据给前台页面)。

![image-20220324102949171](C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220324102949171.png)





### （1）（重要）Spring MVC执行流程

1. 客户端（浏览器）发送请求，直接请求到 `DispatcherServlet`。

2. `DispatcherServlet` 根据请求信息调用 `HandlerMapping`，解析请求对应的 `Handler`。

3. 解析到对应的 `Handler`（也就是我们平常说的 `Controller` 控制器）后，开始由 `HandlerAdapter` 适配器处理。

4. `HandlerAdapter` 会根据 `Handler`来调用真正的处理器开处理请求，并处理相应的业务逻辑。

5. 处理器处理完业务后，会返回一个 `ModelAndView` 对象，`Model` 是返回的数据对象，`View` 是个逻辑上的 `View`。

   > ModelAndView中包含的是“逻辑视图”而非真正的视图对象

6. `ViewResolver` 会根据逻辑 `View` 查找实际的 `View`。

7. 当得到真实的视图对象View后，`DispatcherServlet`就使用这个`View`对象对`ModelAndView`中的模型数据进行视图渲染。

8. 把 `View` 返回给请求者（浏览器）

![image-20220324103027185](C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220324103027185.png)





### （2）常用注解

- @RequestMapping：**用来映射请求地址的**，也就是说将其中的处理器方法映射到url路径上。
- @RequestParam：**将请求参数传递到 控制器的请求方法上，是Spring MVC中的接收普通参数的注解**。
- @RequestBody：如果作用在方法上，就表示该方法的返回结果是直接写入的Http 请求体中（一般在异步获取数据时使用的注解）。
- **@RestController ： = @Controller+@RespenseBody**，**让Handler类中的所有返回值转成json格式，不会返回视图**
- @PathVariable：URL中 的**占位符**可以通过 它绑定到输入参数中。
  - 如：<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220321162058331.png" alt="image-20220321162058331" style="zoom:80%;" />





### （3）Spring MVC的拦截器

​	**拦截器会对Handler处理器进行拦截，这样通过拦截器就可以增强处理器的功能。**Spring MVC中，**所有的拦截器都需要实现HandlerInterceptor接口**，该接口包含如下三个方法：preHandle()、postHandle()、afterCompletion()。

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220324103741277.png" alt="image-20220324103741277" style="zoom:67%;" />

Spring MVC拦截器的执行流程如下：

- 执行preHandle方法，它会返回一个布尔值。**如果为false，则结束所有流程**，如果为true，则执行下一步。
- 执行处理器逻辑，它包含控制器的功能。
- 执行postHandle方法。
- 执行视图解析和视图渲染。
- 执行afterCompletion方法。



#### 过滤器与拦截器的区别？

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220321162645321.png" alt="image-20220321162645321" style="zoom: 80%;" />



# 3. Mybatis

### （1）什么是Mybatis?

​	Mybatis是一个半ORM框架，它内部封装了JDBC，加载驱动、创建连接、创建statement等繁杂的过程，开发者开发时只需要关注如何编写SQL语句，可以严格控制sql执行性能，灵活度高。

> ORM : Object Relational Mapping， 对象关系映射。作用是在关系型数据库和对象之间作一个映射，这样，我们在具体的操作数据库的时候，就不需要再去和复杂的SQL语句打交道，只要像平时操作对象一样操作它就可以了 。

​	**JDBC的核心是 创建statement对象，用statement对象去执行sql语句。**而 在Mybatis中，无需重复创建数据源连接，statement对象。



**映射文件是mybatis的核心**，映射文件即 mapper.xml

**MyBatis主要有两个配置文件：config.xml和Mapper.xml**，当然，这两种配置文件可以自定义文件名。

- config.xml是全局配置文件，主要配置MyBatis的数据源（DataSource），事务管理（TransactionManager），以及打印SQL语句，开启二级缓存，设置实体类别名等功能。
- **Mapper.xml的作用是什么？配置SQL语句。一张表对应一个Mapper文件。**

**Mybatis 一般操作是，用动态代理技术，通过SqlSession对象的getMapper方法 为 Dao接口 生成一个代理对象，操作代理对象调用Dao接口中的方法，从而执行方法中SQL语句。**如：

```java
SqlSession session  = MyBatisUtils.getSqlSession();
StudentDao proxy  = session.getMapper(StudentDao.class);
Student stu = proxy.selectAll();
```





### （2）**#{}和${}的区别是什么？**

- ${}   是**字符串替换**；比如${driver}会被替换为`com.mysql.jdbc. Driver`

- #{}   是**预处理**；

  

Mybatis在处理${}时，就是**把${}直接替换成变量的值**。

而Mybatis在**处理#{}时，会对sql语句进行预处理，将 sql 中的 #{} 替换为 ? 号，调用PreparedStatement的set方法来赋值**；

> - PreparedStatement是Statement的子接口
> - 使用PreparedStatement可以防止SQL注入，所以Mybatis使用#{}也可以防止SQL注入。







### （3）Xml 映射文件中，除了常见的 select|insert|update|delete 标签之外，还有哪些标签？

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220321171504948.png" alt="image-20220321171504948" style="zoom:150%;" />

- <resultMap> ：**可以自定义sql 结果和 Java 对象的映射关系。常用于列名和Java对象属性名不一样的情况。**

  > 下图中， id 是自定义的列名，cid 是实体类的属性名

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220321171824980.png" alt="image-20220321171824980" style="zoom:67%;" />

- <parameterMap>：**用于传入参数，以便匹配SQL语句中的  ?  号,** 跟JDBC中的 PreparedStatement类似

- 动态SQL标签：主要有 if ，where ， foreach ，sql

  > 动态 sql 可以让我们在 Xml 映射文件内，以标签的形式编写动态 sql
  >
  > - 如：
  >
  >   ```Mybatis
  >   <select id="selectUserByUsernameAndSex" resultType="user" parameterType="com.ys.po.User">
  >       select * from user
  >       <where>
  >           <if test="username != null">
  >              username=#{username}
  >           </if>
  >   
  >           <if test="username != null">
  >              and sex=#{sex}
  >           </if>
  >       </where>
  >   </select>
  >   ```
  >
  > 





### （4）(重要)通常一个mapper.xml文件，都会对应一个Dao接口，这个Dao接口的工作原理是什么？

> 理解：一般项目开发中，都是调用 Dao接口中的方法 ，执行sql语句。传统方法中，要调用Dao接口，则需要创建其接口实现类。
>
> 如：<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220321180623993.png" alt="image-20220321180623993" style="zoom:67%;" />

**一个mapper.xml文件，对应一个Mapper接口(Dao接口)，接口中是方法。**调用Dao接口中的方法，可以执行SQL。

如：<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220321180542538.png" alt="image-20220321180542538" style="zoom:50%;" />

**而 Mybatis 采用动态代理，简化了 创建dao接口实现类的过程。**

- **工作原理：Mybatis运行时动态代理为 Dao接口 生成一个代理对象proxy。此时，Mapper 接口是没有实现类的，当调用接口方法时，代理对象会根据 接口类的全限定名+方法名，唯一定位到一个 MapperStatement（即 可以唯一定位到一个SQL标签）。**

- 具体操作：**使用SqlSession对象的方法 getMapper(dao.class)**

  - SqlSession session  = MyBatisUtils.getSqlSession();

    StudentDao dao  = **session.getMapper**(StudentDao.class);
    等同于
    StudentDao dao  = new StudentDaoImpl();

  > - 在 MyBatis 中，每一个 `<select>` 、 `<insert>` 、 `<update>` 、 `<delete>` 标签，都会被解析为一个 `MapperStatement` 对象。
  > - 举例： `com.mybatis3.mappers. StudentDao.findStudentById` ，可以唯一找到 namespace 为 `com.mybatis3.mappers. StudentDao` 下面 `id = findStudentById` 的 方法 ，即找到mapper.xml 中对应的 sql标签语句，就是对应的 `MappedStatement` 。





### （5） MyBatis的xml文件和Mapper接口是怎么绑定的？

​	是**通过**xml文件中，<mapper> 根标签的**namespace属性进行绑定的**，即namespace属性的值需要配置成接口的全限定名称，MyBatis内部就会通过这个值将这个接口与这个xml关联起来。

![image-20220324105843981](C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220324105843981.png)



### （6）Mybatis的Xml映射文件中，不同的Xml映射文件，id是否可以重复？

> - Mybatis的Xml映射文件,即 mapper.xml，一个表对应一个mapper文件。
> - namespace 的值 ，即 mapper接口类(Dao接口)的全限定名

不同的Xml映射文件，如果配置了namespace，那么id可以重复；如果**没有配置namespace，那么id不能重复**；

原因就是**namespace+id是作为Map的key使用的**，如果没有namespace，就剩下id，那么，id重复会导致数据互相覆盖。有了namespace，自然id就可以重复，namespace不同，namespace+id自然也就不同。



### （7）Mybatis是如何进行分页的？分页插件的原理是什么？

- 在MyBatis中，可以通过分页插件实现分页，也可以通过分页SQL自己实现分页。
- **分页插件的原理是，拦截查询SQL，在这个SQL基础上自动为其添加limit分页条件。**它会大大的提高开发的效率，但是无法对分页语句做出有针对性的优化，比如分页偏移量很大的情况，而这些在自己写的分页SQL里却是可以灵活实现的。





### （8）MyBatis缓存机制

MyBatis的缓存分为一级缓存和二级缓存。

- 一级缓存：一级缓存也叫本地缓存，它默认会启用，并且不能关闭。存在于SqlSession的生命周期中。
- 二级缓存：二级缓存存在于SqlSessionFactory 的生命周期中





# 4. SpringBoot

**SpringBoot 就相当于 不需要配置文件的Spring+SpringMVC**。 

**SpringBoot的核心是自动装配**，开发时根据需要，简单配置即可等效于SSM时代中的各种繁琐配置。

**Spring Boot 的核心注解是@SpringBootApplication，它也是启动类使用的注解。**

谈理解 ：![image-20220324110326872](C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220324110326872.png)

> run()  :  run()方法内部执行了两个操作，分别是SpringApplication实例的初始化创建和调用run()启动项目
>
> <img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220323105315829.png" alt="image-20220323105315829" style="zoom:50%;" />





### （1）JavaConfig

- JavaConfig  :   **使用java类作为xml配置文件的替代， 是配置spring   IOC容器的纯java的方式。 在这个java类中可以创建java对象，把对象放入spring容器中（注入到容器）**

  使用到了两个注解：

  - @Configuration ： 放在一个类的上面，表示这个类是作为配置文件使用的。

  - @Bean：在方法上声明对象，把方法返回值注入到容器中。作用相当于<bean>

    ![image-20220321195940910](C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220321195940910.png)

  - @ImporResource：导入其他的xml配置文件。**可以导入老 Spring 项目配置文件。**

​			



### （2）Spring Boot项目是如何导入包的？

通过Spring Boot Starter导入包。

> Spring Boot Starter ：**可以理解为启动器，提供了众多起步依赖来降低项目依赖的复杂度**





### （3）Spring Boot 有哪几种读取配置的方式？

- **@Value ：** 读取比较简单的配置信息

- **@ConfigurationProperties：可以加载一组属性的值**，把配置文件的数据映射为java对象。

  - prefix 属性： 配置文件中的某些key的开头的内容。

    ![image-20220321200908841](C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220321200908841.png)

​		举例：@ConfigurationProperties(prefix = "school")，会去配置文件中读取 以 school 开头的内容

​				application.properties配置文件如下：

```yml
school.name=点
school.website=www.e.com
school.address=兴
```





### （4）Spring Boot 的核心注解是哪个？

Spring Boot 的核心注解是**@SpringBootApplication**，它也是启动类使用的注解，主要包含了 3 个注解：

- **@SpringBootConfiguration**：它组合了 @Configuration 注解，实现**配置文件的功能**。
- **@EnableAutoConfiguration**：**启用 SpringBoot 的自动配置机制**，也可以关闭某个自动配置的选项。
- **@ComponentScan**： **扫描被`@Component` (`@Service`,`@Controller`)注解的 bean，注解默认会扫描该类所在的包下所有的类**。





### （5）（重要） Spring Boot 自动配置原理

**使用Spring Boot时，我们只需引入对应的Starters，Spring Boot启动时便会自动加载相关依赖，这便是Spring Boot的自动配置功能。**

**`@EnableAutoConfiguration` 是实现自动装配的重要注解**

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220324111415476.png" alt="image-20220324111415476" style="zoom: 67%;" />

**整个自动装配的过程是：**

1. Spring Boot通过@EnableAutoConfiguration注解开启自动配置，**读取META-INF/spring.factories 文件，获取所有的 自动配置类。**
2. 当某个 自动配置类 满足其注解`@Conditional`指定的生效条件时，将 该自动配置类 注入到 Spring容器中。
3. 想要自动装配生效必须引入`spring-boot-starter-xxx`包实现起步依赖。

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220323103803428.png" alt="image-20220323103803428" style="zoom:50%;" />

例如：实现自定义线程池

- 第一步，创建`threadpool-spring-boot-starter`工程

<img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220324111016227.png" alt="image-20220324111016227" style="zoom:50%;" />

- 第二步，引入 Spring Boot 相关依赖

  <img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220324111133894.png" alt="image-20220324111133894" style="zoom:50%;" />

- 第三步，创建 自定义线程池 类，并加上`@Conditional`注解

  ![image-20220324111225310](C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220324111225310.png)

- 第四步，在`threadpool-spring-boot-starter`工程的 resources 包下创建`META-INF/spring.factories`文件

- 最后新建工程引入`threadpool-spring-boot-starter`

  <img src="C:\Users\51\AppData\Roaming\Typora\typora-user-images\image-20220324111253335.png" alt="image-20220324111253335" style="zoom:67%;" />





### （6）Spring Boot 配置加载顺序

Spring Boot配置加载顺序优先级是:propertiese文件、YAML文件、系统环境变量、命令行参数。

其中，

- YAML文件 ：  是一种可读的数据序列化语言，它通常用于配置文件。支持数组，数组中的元素可以是基本数据类型或者对象
