* [Spring](#spring)
  * [什么是Spring 以及优点](#%E4%BB%80%E4%B9%88%E6%98%AFspring-%E4%BB%A5%E5%8F%8A%E4%BC%98%E7%82%B9)
  * [Spring Bean生命周期](#spring-bean%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
  * [Spring中bean的作用域](#spring%E4%B8%ADbean%E7%9A%84%E4%BD%9C%E7%94%A8%E5%9F%9F)
  * [Spring IOC](#spring-ioc)
  * [Spring AOP](#spring-aop)
  * [Spring 常用注解](#spring-%E5%B8%B8%E7%94%A8%E6%B3%A8%E8%A7%A3)
  * [Spring事务的传播级别](#spring%E4%BA%8B%E5%8A%A1%E7%9A%84%E4%BC%A0%E6%92%AD%E7%BA%A7%E5%88%AB)
  * [Spring MVC](#spring-mvc)
  * [Spring中设计模式](#spring%E4%B8%AD%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F)
  * [Spring解决循环依赖](https://segmentfault.com/a/1190000015221968)
* [MyBatis](#mybatis)
  * [\#\{\}和$\{\}的区别](#%E5%92%8C%E7%9A%84%E5%8C%BA%E5%88%AB)
  * [Mybatis动态sql是做什么的？都有哪些动态sql？能简述一下动态sql的执行原理不？](#mybatis%E5%8A%A8%E6%80%81sql%E6%98%AF%E5%81%9A%E4%BB%80%E4%B9%88%E7%9A%84%E9%83%BD%E6%9C%89%E5%93%AA%E4%BA%9B%E5%8A%A8%E6%80%81sql%E8%83%BD%E7%AE%80%E8%BF%B0%E4%B8%80%E4%B8%8B%E5%8A%A8%E6%80%81sql%E7%9A%84%E6%89%A7%E8%A1%8C%E5%8E%9F%E7%90%86%E4%B8%8D)
  * [Mybatis是否支持延迟加载？如果支持，它的实现原理是什么？](#mybatis%E6%98%AF%E5%90%A6%E6%94%AF%E6%8C%81%E5%BB%B6%E8%BF%9F%E5%8A%A0%E8%BD%BD%E5%A6%82%E6%9E%9C%E6%94%AF%E6%8C%81%E5%AE%83%E7%9A%84%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86%E6%98%AF%E4%BB%80%E4%B9%88)


Spring 
------------

### 什么是Spring 以及优点

Spring是个轻量ava企业级应用的开源开发框架。Spring主要用来开发Java应用，但是有些扩展是针对构建J2EE平台的web应用。Spring框架目标是简化Java企业级应用开发，并通过POJO为基础的编程模型促进良好的编程习惯。

**优点:**

**控制反转：**
Spring通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找依赖的对象们。

**面向切面的编程(AOP)：**
Spring支持面向切面的编程，并且把应用业务逻辑和系统服务分开。

**容器：**
Spring包含并管理应用中对象的生命周期和配置。

**MVC框架**
：Spring的WEB框架是个精心设计的框架，是Web框架的一个很好的替代品。

**事务管理：**
Spring提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务。

**异常处理：**
Spring提供方便的API把具体技术相关的异常转化为一致的unchecked异常。

### Spring Bean生命周期

Spring 只帮我们管理单例模式 Bean 的完整生命周期，对于 prototype 的 bean ，Spring
在创建好交给使用者之后则不会再管理后续的生命周期。流程如下所示：

![](media/b8c67a9a399aab7ab0d03f39605e5544.png)

Spring的加载流程：https://www.cnblogs.com/xrq730/p/6285358.html

###  Spring中bean的作用域

**默认是单例**

| **作用域** | **字符**  | **描述**                 |
|------------|-----------|--------------------------|
| 单例       | singleton | 整个应用中只创建一个实例 |
| 原型       | prototype | 每次注入时都新建一个实例 |
| 会话       | session   | 为每个会话创建一个实例   |
| 请求       | request   | 为每个请求创建一个实例   |

### Spring IOC

**IOC**:IOC利用Java反射机制，AOP利用代理模式。

IoC（Inverse of Control:控制反转）是一种设计思想，就是 将原本在程序中手动创建对象的控制权，交由Spring框架来管理。 IoC 在其他语言中也有应用，并非 Spirng 特有。 IoC 容器是 Spring 用来实现 IoC 的载体， IoC 容器实际上就是个Map（key，value）,Map 中存放的是各种对象。

将对象之间的相互依赖关系交给 IOC 容器来管理，并由 IOC 容器完成对象的注入。这样可以很大程度上简化应用的开发，把应用从复杂的依赖关系中解放出来。 IOC 容器就像是一个工厂一样，当我们需要创建一个对象的时候，只需要配置好配置文件/注解即可，完全不用考虑对象是如何被创建出来的。 在实际项目中一个 Service 类可能有几百甚至上千个类作为它的底层，假如我们需要实例化这个 Service，你可能要每次都要搞清这个 Service 所有底层类的构造函数，这可能会把人逼疯。如果利用 IOC 的话，你只需要配置好，然后在需要的地方引用就行了，这大大增加了项目的可维护性且降低了开发难度。

**谁控制谁，控制什么：**
传统JavaSE程序设计，直接在对象内部通过new进行创建对象，是程序主动去创建依赖对象；而IoC是有专门一个容器来创建这些对象，即由Ioc容器来控制对
象的创建；谁控制谁？当然是IoC容器控制了对象；控制什么？那就是主要控制了外部资源获取（不只是对象包括比如文件等）。

**为何是反转，哪些方面反转了：**
有反转就有正转，传统应用程序是由自己在对象中主动控制去直接获取依赖对象，也就是正转；而反转则是由容器来帮忙创建及注入依赖对象。为何是反转？因为由容器帮我们查找及注入依赖对象，对象只是被动的接受依赖对象，所以是反转；哪些方面反转了？**依赖对象的获取被反转了。**

**DI—Dependency
Injection，即“依赖注入”：**
组件之间依赖关系由容器在运行期决定，形象的说，即由容器动态的将某个依赖关系注入到组件之中。通过依赖注入机制，只需要通过简单的配置，而无需任何代码就可指定目标需要的资源，就能完成自身的业务逻辑，而**不需要关心具体的资源来自何处，由谁实现**。

谁依赖于谁：当然是应用程序依赖于IoC容器；

为什么需要依赖：应用程序需要IoC容器来提供对象需要的外部资源；

谁注入谁：很明显是IoC容器注入应用程序某个对象，应用程序依赖的对象；

注入了什么：就是注入某个对象所需要的外部资源（包括对象、资源、常量数据）。

**IoC和DI其实是同一个概念的不同角度描述，**
由于控制反转概念比较含糊（可能只是理解为容器控制对象这一个层面，很难让人想到谁来维护对象关系），所以2004年大师级人物MartinFowler又给出了一个新的名字：“依赖注入”，相对IoC而言，“依赖注入”明确描述了“被注入对象依赖IoC容器配置依赖对象”**。

**解决循环依赖：**
https://blog.csdn.net/u010853261/article/details/77940767*


### Spring AOP

AOP技术利用一种称为“横切”的技术，剖解开封装的对象内部，并将那些影响了多个类的公共行为封装到一个可重用模块，**并将其名为“Aspect”，即切面。**所谓“切面”，简单地说，就是将那些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可操作性和可维护性。使用“横切”技术，*AOP*把软件系统分为两个部分：核心关注点和横切关注点。业务处理的主要流程是核心关注点，与之关系不大的部分是横切关注点。横切关注点的一个特点是，他们经常发生在核心关注点的多处，而各处都基本相似。比如权限认证、日志、事务处理。

AOP的作用在于分离系统中的各种关注点，将核心关注点和横切关注点分离开来。

纵观AOP编程，程序员只需要**参与三个部分：**

定义普通业务组件

定义切入点，一个切入点可能横切多个业务组件

定义增强处理，增强处理就是在AOP框架为普通业务组件织入的处理动作

所以进行AOP编程的关键就是**定义切入点和定义增强处理**，一旦定义了合适的切入点和增强处理，

AOP框架将自动生成AOP代理，即：**代理对象的方法=增强处理+被代理对象的方法。**

AOP是通过动态代理来实现的，Spring创建代理的规则为：

>   **默认使用JDK动态代理**来创建AOP代理，这样就可以为任何**接口实例**创建代理了

>   当需要代理的类不是代理接口的时候，Spring会切换为使用CGLIB代理，也可强制使用CGLIB。

**动态代理**：http://www.importnew.com/27772.html

如果使用spring mvc，那post请求跟put请求有什么区别啊

### Spring 常用注解

**\@Component：**
标准一个普通的spring Bean类

**\@Repository：**
标注一个DAO组件类。

**\@Service：**
标注一个业务逻辑组件类。

**\@Controller：**
标注一个控制器组件类。

**\@Autowired：**
依赖注入，只按照Type 注入

**\@Qualifier：**
当容器中存在同样类型的多个bean时，可以使用 \@Qualifier
注解指定注入Bean 的名称

\@Resource：\@Resource 的作用相当于 \@Autowired，只不过\@Resource
默认按byName自动注入

**\@PostConstruct \@PreDestroy**
方法：实现初始化和销毁bean之前进行的操作
**\@RequestMapping
：这**个注解用于将url映射到整个处理类或者特定的处理请求的方法。如\@RequestMapping(value=”/haha”,method=RequestMethod.GET)

**\@RequestParam：**
将请求的参数绑定到方法中的参数上，有required参数，默认情况下，required=true，也就是改参数必须要传。如果改参数可以传可不传，可以配置
required=false。

**\@RequestBody** 
： \@RequestBody是指方法参数应该被绑定到HTTP请求Body上。\@ResponseBody在输出JSON格式的数据时，会经常用到。

**\@Transactional：**
事务注解。

**\@Component 和 \@Bean 的区别：**
* 作用对象不同: @Component 注解作用于类，而\@Bean注解作用于方法。
* \@Component通常是通过类路径扫描来自动侦测以及自动装配到Spring容器中（可以使用 \@ComponentScan 注解定义要扫描的路径从中找出标识了需要装配的类自动装配到 Spring 的 bean 容器中）。\@Bean 注解通常是在标有该注解的方法中定义产生这个 bean,\@Bean告诉了Spring这是某个类的示例，当我需要用它的时候还给我。

### Spring事务的传播级别

| 事务传播行为类型	 | 说明 |
| :------:| :------ |
| PROPAGATION_REQUIRED |	如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中 |
| PROPAGATION_SUPPORTS |	支持当前事务，如果当前没有事务，就以非事务方式执行 |
| PROPAGATION_MANDATORY |	使用当前的事务，如果当前没有事务，就抛出异常 |
| PROPAGATION_REQUIRES_NEW |	新建事务，如果当前存在事务，把当前事务挂起 |
| PROPAGATION_NOT_SUPPORTED |	以非事务方式执行操作，如果当前存在事务，就把当前事务挂起 |
| PROPAGATION_NEVER |	以非事务方式执行，如果当前存在事务，则抛出异常 |
| PROPAGATION_NESTED |	如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与PROPAGATION_REQUIRED类似的操作 |


###  Spring MVC

Spring
MVC是一个基于MVC架构的用来简化web应用程序开发的应用开发框架，它是Spring的一个模块,无需中间整合层来整合，属于表现层的框架。在web模型中，MVC是一种很流行的框架，通过把Model，View，Controller分离，把较为复杂的web应用分成逻辑清晰的几部分，简化开发，减少出错，方便组内开发人员之间的配合。SpringMVC的核心架构如下：

![](media/7bb35a1120e5eb25745317d009811937.png)

具体流程：

（1）首先用户发送请求——\>DispatcherServlet，前端控制器收到请求后自己不进行处理，而是委托给其他的解析器进行处理，作为统一访问点，进行全局的流程控制。

（2）DispatcherServlet——\>HandlerMapping，映射处理器将会把请求映射为HandlerExecutionChain对象（包含一个Handler处理器（页面控制器）对象、多个HandlerInterceptor拦截器）对象。

（3）DispatcherServlet——\>HandlerAdapter，处理器适配器将会把处理器包装为适配器，从而支持多种类型的处理器，即适配器设计模式的应用，从而很容易支持很多类型的处理器。

（4）HandlerAdapter——\>调用处理器相应功能处理方法，并返回一个ModelAndView对象（包含模型数据、逻辑视图名）。

（5）ModelAndView对象（Model部分是业务对象返回的模型数据，View部分为逻辑视图名）—\>
ViewResolver， 视图解析器将把逻辑视图名解析为具体的View。

（6）View——\>渲染，View会根据传进来的Model模型数据进行渲染，此处的Model实际是一个Map数据结构。

（7）返回控制权给DispatcherServlet，由DispatcherServlet返回响应给用户，到此一个流程结束。

### Spring中设计模式

**第一种：简单工厂**

简单工厂模式的实质是由一个工厂类根据传入的参数，动态决定应该创建哪一个产品类。Spring中的BeanFactory就是简单工厂模式的体现，根据传入一个唯一的标识来获得bean对象，但是否是在传入参数后创建还是传入参数前创建这个要根据具体情况来定。、

**第二种：单例模式（Singleton）**

保证一个类仅有一个实例，并提供一个访问它的全局访问点。Spring中的单例模式完成了后半句话，即提供了全局的访问点BeanFactory。但没有从构造器级别去控制单例，这是因为spring管理的是是任意的Java对象。

**Spring下默认的bean均为singleton，可以通过singleton=“true\|false”来指定。**

**第三种：适配器（Adapter）**

在Spring的Aop中，使用的Advice（通知）来增强被代理类的功能。Spring实现这一AOP功能的原理就使用代理模式（1、JDK动态代理。2、CGLib字节码生成技术代理。）对类进行方法级别的切面增强，即生成被代理类的代理类，
并在代理类的方法前，设置拦截器，通过执行拦截器重的内容增强了代理方法的功能，实现的面向切面编程。

**第四种：代理（Proxy）**

为其他对象提供一种代理以控制对这个对象的访问。从结构上来看和Decorator模式类似，但Proxy是控制，更像是一种对功能的限制，而Decorator是增加职责。Spring的Proxy模式在aop中有体现，比如JdkDynamicAopProxy和Cglib2AopProxy。

**第五种：观察者（Observer）**

定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。Spring中Observer模式常用的地方是listener的实现。如ApplicationListener。


MyBatis
------------

MyBatis是一款优秀的持久层框架，它支持**定制化SQL**、存储过程以及高级映射。MyBatis避免了几乎所有的JDBC代码和手动设置参数以及获取结果集。MyBatis可以使用简单的XML或注解来配置和映射原生信息，将接口和Java的POJOs(Plain
Old Java Objects,普通的Java对象)映射成数据库中的记录。

MyBatis的强大特性之一便是**它的**[动态SQL](http://www.mybatis.org/mybatis-3/zh/dynamic-sql.html)**。**

### \#{}和\${}的区别

**\#{
}：占位符，**
防止sql注入,使用\#{}格式的语法在mybatis中使用Preparement语句来安全的设置值

如：order by \#{user_id}

如果传入的值是111,那么解析成sql时的值为order by "111"

如果传入的值是id，则解析成的sql为order by "id"

**\${ }：sql拼接符号,**
将传入的数据直接显示生成在sql中(order by)

如：order by \${user_id}

如果传入的值是111,那么解析成sql时的值为order by 111

如果传入的值是id，则解析成的sql为order by id



### Mybatis动态sql是做什么的？都有哪些动态sql？能简述一下动态sql的执行原理不？

答：Mybatis动态sql可以让我们在Xml映射文件内，以标签的形式编写动态sql，完成逻辑判断和动态拼接sql的功能，Mybatis提供了9种动态sql标签**trim\|where\|set\|foreach\|if\|choose\|when\|otherwise\|bind**。

其执行原理为，使用OGNL从sql参数对象中计算表达式的值，根据表达式的值动态拼接sql，以此来完成动态sql的功能。

### Mybatis是否支持延迟加载？如果支持，它的实现原理是什么？

答：Mybatis仅支持association关联对象和collection关联集合对象的延迟加载，association指的就是一对一，collection指的就是一对多查询。在Mybatis配置文件中，可以配置是否启用延迟加载lazyLoadingEnabled=true\|false。

它的原理是，使用CGLIB创建目标对象的代理对象，当调用目标方法时，进入拦截器方法，比如调用a.getB().getName()，拦截器invoke()方法发现a.getB()是null值，那么就会单独发送事先保存好的查询关联B对象的sql，把B查询上来，然后调用a.setB(b)，于是a的对象b属性就有值了，接着完成a.getB().getName()方法的调用。这就是延迟加载的基本原理。


