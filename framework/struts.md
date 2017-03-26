## 描述 Struts 体系结构？对应各个部分的开发工作主要包括哪些？
Struts开源架构很好的实现了MVC模式，MVC即Model-View-Controller的缩写，是一种常用的设计模式。MVC 减弱了业务逻辑接口和数据接口之间的耦合，以及让视图层更富于变化。在Struts2的模型 - 视图 - 控制器模式，实现以下五个核心部件：
    Actions
    Interceptors
    Value Stack / OGNL
    Results / Result types
    View technologies

Struts 2 从传统的MVC框架操作需要的模型，而不是在控制器角色略有不同，虽然有一些重叠
**模型**
模型以一个或多个java bean的形式存在。这些bean分为三类：Action Form、Action、JavaBean or EJB。Action Form通常称之为FormBean，封装了来自于Client的用户请求信息，如表单信息。Action通常称之为ActionBean，获取从ActionSevlet传来的FormBean，取出FormBean中的相关信息，并做出相关的处理，一般是调用Java Bean或EJB等。

**视图**
主要由JSP生成页面完成视图，Struts提供丰富的JSP 标签库： Html，Bean，Logic，Template等

**控制器**
该控制器负责响应用户输入和执行数据模型对象的相互作用。控制器接收输入，验证输入，然后进行业务操作，修改数据模型的状态。

这个解释比较好：　http://www.yiibai.com/struts_2/struts_architecture.html
http://www.cnblogs.com/langtianya/archive/2013/04/09/3011090.html

## 什么是Struts2
Apache Struts2的是一个在Java中构建**Web应用程序开源框架**。 Struts2是基于OpenSymphony的WebWork的框架。它是Struts1的提高，它更加灵活，易于使用和扩展。 Struts2的核心组成部分是Action，拦截器和结果页。

Struts2提供了许多方法来创建Action类，并**通过struts.xml中或通过注释进行配置**。我们可以创建自己的**拦截器**实现常见任务。 Struts2中自带了很多的**标签，并使用OGNL表达式语言**。我们可以创造我们自己的类型转换器来呈现的结果页面。结果页面可以JSP和FreeMarker的模板。

## Struts 与webWork的区别
Struts 2是Struts的下一代产品。是在 struts 和WebWork的技术基础上进行了合并，全新的Struts 2框架。其全新的Struts 2的体系结构与Struts 1的体系结构的差别巨大。Struts 2以WebWork为核心，采用拦截器的机制来处理用户的请求，这样的设计也使得业务逻辑控制器能够与Servlet API完全脱离开，所以Struts 2可以理解为WebWork的更新产品。

Struts和Webwork同为服务于Web的一种MVC框架，从某种程度上看，Struts2是从WebWork2上升级得到的。甚至Apache的官方文档也讲：WebWork2到Struts2是平滑的过渡。我们甚至也可以说Struts2就是WebWork2.3而已。在很多方面Struts仅仅是改变了WebWork下的名称。Struts2对应的有自己的标签，并且功能强大。Webwork也有自己的标签。
1) 在很多方面Struts2仅仅是改变了WebWork下的名称,如DispatcherUtil 改为了Dispatcher. 
2) AroundInterceptor：Struts 2不再支持WebWork中的AroundInterceptor。如果应用程序中需要使用AroundInterceptor，则应该自己手动导入WebWork中的AroundInterceptor类。
3) IoC容器支持：Struts 2不再支持内建的IoC容器，而改为全面支持Spring的IoC容器，以Spring的IoC容器作为默认的Object工厂。
4) 富文本编辑器标签：Struts 2不再支持WebWork的富文本编辑器，如果应用中需要使用富文本编辑器，则应该使用**Dojo**的富文本编辑器。

http://developer.51cto.com/art/201106/271744.htm

## struts2 与struts1的区别
Action类,线程安全，测试，标签，验证
<img src="imgs/struts2-vs-struts1.png">

**Action 类:**
• Struts1要求Action类继承一个**抽象**基类。Struts1的一个普遍问题是使用抽象类编程而不是接口，而struts2的Action是接口。
• Struts 2 Action类可以实现一个Action**接口**，也可实现其他接口，**使可选和定制的服务成为可能**。Struts2提供一个ActionSupport基类去 实现 常用的接口。Action接口不是必须的，任何有execute标识的POJO对象都可以用作Struts2的Action对象。

**线程模式**:
• Struts1 Action是单例模式并且必须是线程安全的，因为仅有Action的一个实例来处理所有的请求。单例策略限制了Struts1 Action能作的事，并且要在开发时特别小心。Action资源必须是线程安全的或同步的。(**不是线程安全**)
• Struts2 Action对象为每一个请求产生一个实例，因此没有线程安全问题。（实际上，servlet容器给每个请求产生许多可丢弃的对象，并且不会导致性能和垃圾回收问题）(**线程安全**)

**Servlet 依赖**:
• Struts1 Action 依赖于Servlet API ,因为当一个Action被调用时HttpServletRequest 和 HttpServletResponse 被传递给execute方法。
• Struts 2 Action不依赖于容器，允许Action脱离容器单独被测试。如果需要，Struts2 Action仍然可以访问初始的request和response。但是，其他的元素减少或者消除了直接访问HttpServetRequest 和 HttpServletResponse的必要性。

**可测性**:
• 测试Struts1 Action的一个主要问题是execute方法暴露了servlet API（这使得测试要依赖于容器）。一个第三方扩展－－Struts TestCase－－提供了一套Struts1的模拟对象（来进行测试）。
• Struts 2 Action可以通过初始化、设置属性、调用方法来测试，“依赖注入”支持也使测试更容易。

**表达式语言**：
• Struts1 整合了JSTL，因此使用JSTL EL。这种EL有基本对象图遍历，但是对集合和索引属性的支持很弱。
• Struts2可以使用JSTL，但是也支持一个更强大和灵活的表达式语言－－"Object Graph Notation Language" (OGNL). 

**绑定值到页面（view）**:
• Struts 1使用标准JSP机制把对象绑定到页面中来访问。
• Struts 2 使用 "ValueStack"技术，使taglib能够访问值而不需要把你的页面（view）和对象绑定起来。ValueStack策略允许通过一系列名称相同但类型不同的属性重用页面（view）。

## Struts工作流程:
![图片标题](http://www.evget.com/images/article/08072801.png)
Struts2:
(1)客户端提交一个HttpServletRequest请求(.action或JSP页面)
(2)请求被提交到一系列（主要是三层）的过滤器（Filter），如（ActionContextCleanUp、其他过滤器（SiteMesh等）、 FilterDispatcher）。注意这里是有顺序的，先ActionContextCleanUp，再其他过滤器（SiteMesh等）、最后到FilterDispatcher。 
(3)FilterDispatcher是Struts2控制器的核心,它通常是过滤器链中的最后一个过滤器,FilterDispatcher进行初始化并启用核心doFilter().FilterDispatcher询问ActionMapper是否需要调用某个Action来处理这个（request）请求，如果ActionMapper决定需要调用某个Action，FilterDispatcher把请求的处理交给ActionProxy。 
(6)ActionProxy通过**ConfigurationManager**(它会访问struts.xml)询问框架的配置文件,找到需要调用的Action类.
(7)ActionProxy创建一个ActionInvocation实例,而ActionInvocation通过代理模式调用Action,(在调用之前会根据配置文件加载相关的所有Interceptor拦截器)
(8)Action执行完毕后,返回一个result字符串,此时再按相反的顺序通过Interceptor拦截器.
(9)最后ActionInvocation负责根据struts.xml中配置的result元素,找到与返回值对应的result,决定进行下一步输出.

## 为什么要使用 Struts2 & Struts2 的优点：
①. 基于 MVC 架构，框架结构清晰。
②. 使用 OGNL: OGNL 可以快捷的访问值栈中的数据、调用值栈中对象的方法
③. 拦截器: Struts2 的拦截器是一个 Action 级别的 AOP, Struts2 中的许多特性都是通过拦截器来实现的, 例如异常处理，文件上传，验证等。拦截器是可配置与重用的
④. 多种表现层技术. 如：JSP、FreeMarker、Velocity 等

## SpringMVC 与　Struts2区别
1.核心控制器（前端控制器、预处理控制器）：对于使用过mvc框架的人来说这个词应该不会陌生，核心控制器的主要用途是处理所有的请求，然后对那些特殊的请求 （控制器）统一的进行处理(字符编码、文件上传、参数接受、异常处理等等),spring mvc核心控制器是Servlet，而Struts2是Filter。
2.控制器实例：Spring Mvc会比Struts快一些（理论上）。Spring Mvc是基于方法设计，而Sturts是基于对象，每次发一次请求都会实例一个action，每个action都会被注入属性，而Spring更像Servlet一样，只有一个实例，每次请求执行对应的方法即可(注意：由于是单例实例，所以应当避免全局变量的修改，这样会产生线程安全问题)。
3. 管理方式：大部分的公司的核心架构中，就会使用到spring,而spring mvc又是spring中的一个模块，所以spring对于spring mvc的控制器管理更加简单方便，而且提供了全 注解方式进行管理，各种功能的注解都比较全面，使用简单，而struts2需要采用XML很多的配置参数来管理（虽然也可以采用注解，但是几乎没有公司那 样使用）。
4.参数传递：Struts2中自身提供多种参数接受，其实都是通过（ValueStack）进行传递和赋值，而SpringMvc是通过方法的参数进行接收。
5.intercepter 的实现机制：struts有以自己的interceptor机制，spring mvc用的是独立的AOP方式。这样导致struts的配置文件量还是比spring mvc大，虽然struts的配置能继承，所以我觉得论使用上来讲，spring mvc使用更加简洁，开发效率Spring MVC确实比struts2高。spring mvc是方法级别的拦截，一个方法对应一个request上下文，而方法同时又跟一个url对应，所以说从架构本身上spring3 mvc就容易实现restful url。struts2是类级别的拦截，一个类对应一个request上下文；实现restful url要费劲，因为struts2 action的一个方法可以对应一个url；而其类属性却被所有方法共享，这也就无法用注解或其他方式标识其所属方法了。spring3 mvc的方法之间基本上独立的，独享request response数据，请求数据通过参数获取，处理结果通过ModelMap交回给框架方法之间不共享变量，而struts2搞的就比较乱，虽然方法之间 也是独立的，但其所有Action变量是共享的，这不会影响程序运行，却给我们编码，读程序时带来麻烦。
7.spring mvc处理ajax请求,直接通过返回数据，方法中使用注解@ResponseBody，spring mvc自动帮我们对象转换为JSON数据。


## Filter,Listener,Servlet区别
1) Filter　实现javax.servlet.Filter接口，在web.xml中配置与标签指定使用哪个Filter实现类过滤哪些URL链接。只在web启动时进行初始化操作。filter流程是**线性**的， url传来之后，检查之后，可保持原来的流程继续向下执行，被下一个filter, servlet接收等，而servlet 处理之后，不会继续向下传递。filter功能可用来保持流程继续按照原来的方式进行下去，或者主导流程，而servlet的功能主要用来主导流程。
特点：可以在响应之前修改Request和Response的头部，只能转发请求，不能直接发出响应。filter可用来进行字符编码的过滤，检测用户是否登陆的过滤，禁止页面缓存等
2) Servlet 流程是短的，url传来之后，就对其进行处理，之后返回或转向到某一自己指定的页面。它主要用来在业务处理之前进行控制。
3) Listener
servlet,filter都是针对url之类的，而listener是针对**对象操作**的，如session的创建，session.setAttribute的发生，在这样的事件发
生时做一些事情。

## Struts2拦截器和过滤器的区别
①、过滤器依赖于Servlet容器，而拦截器不依赖于Servlet容器。
②、Struts2 拦截器只能对 Action 请求起作用，而过滤器则可以对几乎所有请求起作用。
③、拦截器可以访问 Action 上下文(ActionContext)、值栈里的对象(ValueStack)，而过滤器不能.
④、在 Action 的生命周期中，拦截器可以多次调用，而过滤器只能在容器初始化时被调用一次。

## Struts的Action是不是线程安全的
 struts2的action是线程安全的，struts1的action不是线程安全的 。
对于struts1 ，Action是单例模式，一个实例来处理所有的请求。当第一次**.do的请求过来时，在内存中的actionmapping中找到相对应的action，然后new出这个action放在缓存中，当第二次一样的请求过来时，还是找的这个action，所以对于struts1来说，action是单实例的，只有一个，如果在action中定义变量，就要非常小心了，因为并发问题，可能带来灾难性的后果，也不是不可以，我们可以加锁达到同步，只是在性能上就 要折衷了。
声明局部变量，或者扩展RequestProcessor，让每次都创建一个Action，或者在spring中用scope=”prototype”来管理，不申明类变量就可以保证线程安全。因为只存在一个Action类实例，所有线程会共享类变量。
struts2 在struts1的基础上做了改进 ，对于struts2 ，每次请求过来都会new一个新的action, 所以说struts2的action没有线程安全问题，是线程安全的，但同时也带来一个问题，每次都new一个action ，这样action的实例太多 ， 在性能方面还是存在一定的缺陷的。

## struts2.0的mvc模式与struts1.0的区别?
与struts1最大的不同是：struts2的控制器。struts2的控制器不再像struts1的控制器,需要**继承**一个Action父类,甚至可以无需实现任何接口,struts2的Action就是一个普通的POJO。实际上，Struts2 的Action就是一个包含execute方法的普通Java类该类里包含的多个属性用于封装用户的请求参数。

## 你对MVC的理解，MVC有什么优缺点？结合Struts，说明在一个Web应用如何去使用？
MVC设计模式（应用观察者模式的框架模式）
M: Model(Business process layer)，模型，是应用程序中用于处理应用程序数据逻辑的部分,通常模型对象负责在数据库中存取数据。,并独立于表现层 (Independent of presentation)。
V: View(Presentation layer)，视图，通过客户端数据类型显示数据,并回显模型层的执行结果。
C: Controller(Control layer)，控制器，也就是**视图层和模型层桥梁**，控制数据的流向，接受视图层发出的事件，并重绘视图
优点：
1）视图控制模型分离，提高代码重用性。
2）提高开发效率。
3）便于后期维护，降低维护成本。
4）方便多开发人员间的分工。
5)结构清晰
缺点：
1）清晰的构架以代码的复杂性为代价， 对小项目优可能反而降低开发效率。
2）运行效率相对较低 

MVC框架的一种实现模型
模型二(Servlet-centric)： JSP+Servlet+JavaBean，以控制为核心，JSP只负责显示和收集数据，Sevlet，连接视图和模型，将视图层数据，发送给模型层，JavaBean，分为业务类和数据实体，业务类处理业务数据，数据实体，承载数据，基本上大多数的项目都是使用这种MVC的实现模式。

Struts MVC框架(Web application frameworks)  :Struts是使用MVC的实现模式二来实现的，也就是以控制器为核心。

Struts提供了一些组件使用MVC开发应用程序：
Model：Struts 没有提供model 类。这个商业逻辑必须由Web 应用程序的开发者以JavaBean或EJB的形式提供
View：Struts提供了action form创建form bean, 用于在controller和view间传输数据。此外，Struts提供了自定义JSP标签库，辅助开发者用JSP创建交互式的以表单为基础的应用程序，应用程序资源文件保留了一些文本常量和错误消息，可转变为其它语言， 可用于JSP中。
Controller：Struts提供了一个核心的控制器FilterDispatcher，通过这个核心的控制器来调用其他用户注册了的自定义的控制器Action，自定义Action需要符合Struts的自定义Action规范，还需要在struts-config.xml的特定配置文件中进行配置，接收JSP输入字段形成Action form，然后调用一个Action控制器。Action控制器中提供了model的逻辑接口。

## 说出 struts2 中至少 5 个的默认拦截器
exception；fileUpload；i18n；modelDriven；params；prepare；token；tokenSession；validation 等
拦截器的生命周期与工作过程 ?

## 每个拦截器都是需要实现 Interceptor 接口
init()：在拦截器被创建后立即被调用, 它在拦截器的生命周期内只被调用一次. 可以在该方法中对相关资源进行必要的初始化；
intercept(ActionInvocation invocation)：每拦截一个动作请求，该方法就会被调用一次；
destroy：该方法将在拦截器被销毁之前被调用, 它在拦截器的生命周期内也只被调用一次；
  
## 创建Action类有几种方法？
实现Action 接口
使用Struts2 @Action 元注解
继承ActionSupport类，必须实现 execute() 方法，返回一个可配置的字符串

##Struts2的拦截器执行什么模式？ 
**责任链模式**
过滤器decorator模式和职责链模式

