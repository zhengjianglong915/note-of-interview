## MyBatis 与 Hibernate的区别


## JDBC 和　Hibernate的区别


## Hibernate的原理体系架构
![图片标题](http://my.csdn.net/uploads/201205/30/1338346660_4642.gif)
**1) SessionFactory (org.hibernate.SessionFactory)**
针对单个数据库映射关系经过编译后的内存镜像，是线程安全的（不可变）。它是生成Session的工厂，本身要用到ConnectionProvider。该对象可以在进程或集群的级别上，为那些事务之间可以重用的数据提供可选的二级缓存。
**2) Session (org.hibernate.Session)**
表示应用程序与持久储存层之间交互操作的一个单线程对象，此对象生存期很短。 其隐藏了JDBC连接，也是Transaction的工厂。其会持有一个针对持久化对象的必选（第一级）缓存，在遍历对象图或者根据持久化标识查找对象时会用到。
**3)事务Transaction (org.hibernate.Transaction)**
（可选的）应用程序用来指定原子操作单元范围的对象，它是单线程的，生命周期很短。 它通过抽象将应用从底层具体的JDBC、JTA以及CORBA事务隔离开。 某些情况下，一个Session之内可能包含多个Transaction对象。 尽管是否使用该对象是可选的，但无论是使用底层的API还是使用Transaction对象，事务边界的开启与关闭是必不可少的。
**4) ConnectionProvider  (org.hibernate.connection.ConnectionProvider)**
（可选的）生成JDBC连接的工厂（同时也起到连接池的作用）。 它通过抽象将应用从底层的Datasource或DriverManager隔离开。 仅供开发者扩展/实现用，并不暴露给应用程序使用。
**5)TransactionFactory (org.hibernate.TransactionFactory)**
（可选的）生成Transaction对象实例的工厂。 仅供开发者扩展/实现用，并不暴露给应用程序使用。
**6) 持久的对象及其集合**
带有持久化状态的、具有业务功能的单线程对象，此对象生存期很短。 这些对象可能是普通的JavaBeans/POJO，唯一特殊的是他们正与（仅仅一个）Session相关联。 一旦这个Session被关闭，这些对象就会脱离持久化状态，这样就可被应用程序的任何层自由使用。 （例如，用作跟表示层打交道的数据传输对象。）
**7) 瞬态(transient)和脱管(detached)的对象及其集合**
那些目前没有与session关联的持久化类实例。他们可能是在被应用程序实例化后，尚未进行持久化的对象。也可能是因为实例化他们的Session已经被关闭而脱离持久化的对象。

参考: http://blog.sina.com.cn/s/blog_667fe4a501016awl.html

## 五大核心接口，
Hibernate的核心接口一共有5个，分别为:Session、SessionFactory、Transaction、Query和 Configuration。这5个核心接口在任何开发中都会用到。通过这些接口，不仅可以对持久化对象进行存取，还能够进行事务控制。下面对这五的核心 接口分别加以介绍。
**1) Session接口**:
Session接口负责执行被持久化对象的CRUD操作(CRUD的任务是完成与数据库的交流，包含了很多常见的 SQL语句。)。但需要注意的是Session对象是非线程安全的。同时，Hibernate的session不同于JSP应用中的HttpSession。这里当使用session这个术语时，其实指的是Hibernate中的session，而以后会将HttpSesion对象称为用户session。
**2) SessionFactory接口**
SessionFactroy接口负责初始化Hibernate。它充当数据存储源的代理，并负责创建 Session对象。这里用到了工厂模式。需要注意的是SessionFactory并不是轻量级的，因为一般情况下，一个项目通常只需要一个SessionFactory就够，当需要操作多个数据库时，可以为每个数据库指定一个SessionFactory。
**3) Configuration接口**
Configuration接口负责配置并启动Hibernate，创建SessionFactory对 象。在Hibernate的启动的过程中，Configuration类的实例首先定位映射文档位置、读取配置，然后创建SessionFactory对象。
**4) Transaction接口**
Transaction接口负责事务相关的操作。它是可选的，开发人员也可以设计编写自己的底层事务处理代码。
**5) Query和Criteria接口**:
Query和Criteria接口负责执行各种数据库查询。它可以使用HQL语言或SQL语句两种表达方式。

http://blog.csdn.net/martinmateng/article/details/50879436

## Hibernate对象的三种状态转换
### 1.瞬时状态 (transient)
特征：
1.不处于Session 缓存中
2.数据库中没有对象记录
Java如何进入临时状态
1.通过new语句刚创建一个对象时
2.当调用Session 的delete()方法，从Session 缓存中删除一个对象时。

### 2.持久化状态(persisted)
特征：
1.处于Session 缓存中
2.持久化对象数据库中设有对象记录
3.Session 在特定时刻会保持二者同步
Java如何进入持久化状态
1.Session 的save()把临时－》持久化状态
2.Session 的load(),get()方法返回的对象
3.Session 的find()返回的list集合中存放的对象
4.ession 的update(),saveOrupdate()使游离－》持久化

### 3.游离状态(detached)
特征：
1.不再位于Session 缓存中
2.游离对象由持久化状态转变而来，数据库中可能还有对应记录。
Java如何进入持久化状态－》游离状态
1.Session 的close()方法
2.Session 的evict()方法，从缓存中删除一个对象。提高性能。少用。

## 事务管理

## Hibernate对一二级缓存的使用
（1）一级缓存就是Session级别的缓存，一个Session做了一个查询操作，它会把这个操作的结果放在一级缓存中，如果短时间内这个session（一定要同一个session）又做了同一个操作，那么hibernate直接从一级缓存中拿，而不会再去连数据库，取数据；
（2）二级缓存就是SessionFactory级别的缓存，顾名思义，就是查询的时候会把查询结果缓存到二级缓存中，如果同一个sessionFactory创建的某个session执行了相同的操作，hibernate就会从二级缓存中拿结果，而不会再去连接数据库；
3）Hibernate中提供了两级Cache，第一级别的缓存是Session级别的缓存，它是属于事务范围的缓存。这一级别的缓存由hibernate管理的，一般情况下无需进行干预；第二级别的缓存是SessionFactory级别的缓存，它是属于进程范围或群集范围的缓存。这一级别的缓存可以进行配置和更改，并且可以动态加载和卸载。Hibernate还为查询结果提供了一个查询缓存，它依赖于第二级缓存；

http://www.open-open.com/lib/view/open1413527015465.html

## Lazy-Load的理解
在Hibernate框架中，当我们要访问的数据量过大时，明显用缓存不太合适， 因为内存容量有限，为了减少并发量，减少系统资源的消耗，这时Hibernate用懒加载机制来弥补这种缺陷，但是这只是弥补而不是用了懒加载总体性能就提高了。我们所说的懒加载也被称为延迟加载，它在查询的时候不会立刻访问数据库，而是返回代理对象，当真正去使用对象的时候才会访问数据库。
1、通过Session.load()实现懒加载
load(Object, Serializable)：根据id查询。查询返回的是代理对象，不会立刻访问数据库，是懒加载的。当真正去使用对象的时候才会访问数据库。用load()的时候会发现不会打印出查询语句，而使用get()的时候会打印出查询语句。
使用load()时如果在session关闭之后再查询此对象，会报异常：could not initialize proxy - no Session。处理办法：在session关闭之前初始化一下查询出来的对象：Hibernate.initialize(user);使用load()可以提高效率，因为刚开始的时候并没有查询数据库。但很少使用。

2、one-to-one(元素)实现了懒加载。
在一对一的时候，查询主对象时默认不是懒加载。即：查询主对象的时候也会把从对象查询出来。需要把主对象配制成lazy="true" constrained="true" fetch="select"。此时查询主对象的时候就不会查询从对象，从而实现了懒加载。一对一的时候，查询从对象的是默认是懒加载。即：查询从对象的时候不会把主对象查询出来。而是查询出来的是主对象的代理对象。

3、many-to-one（元素）实现了懒加载。
多对一的时候，查询主对象时默认是懒加载。即：查询主对象的时候不会把从对象查询出来。

4、one-to-many(元素)懒加载：默认会懒加载，这是必须的，是重常用的。
一对多的时候，查询主对象时默认是懒加载。即：查询主对象的时候不会把从对象查询出来。

参考： http://blog.csdn.net/sanjy523892105/article/details/7071139
http://blog.csdn.net/yaorongwang0521/article/details/7074573

## 悲观锁和乐观锁
**悲观锁 (Pessimistic Locking)**
悲观锁，正如其名，他是对数据库而言的，数据库悲观了，他感觉每一个对他操作的程序都有可能产生并发。它指的是对数据被外界（包括本系统当前的其他事务，以及来自外部系统的事务处理）修改持保守态度，因此，在整个数据处理过程中，将数据处于锁定状态。悲观锁的实现，往往依靠数据库提供的锁机制（也只有数据库层提供的锁机制才能真正保证数据访问的排他性，否则，即使在本系统中实现了加锁机制，也无法保证外部系统不会修改数据）

优点：数据的一致性保持得很好
缺点：不适合多个用户并发访问。

**乐观锁**
 相对悲观锁而言，乐观锁机制采取了更加宽松的加锁机制。悲观锁大多数情况下依靠数据库的锁机制实现，以保证操作最大程度的独占性。但随之而来的就是数据库性能的大量开销，特别是对长事务而言，这样的开销往往无法承受。乐观锁机制在一定程度上解决了这个问题。乐观锁，大多是基于数据版本（Version）记录机制实现。何谓数据版本？即为数据增加一个版本标识，在基于数据库表的版本解决方案中，一般是通过为数据库表增加一个"version"字段来实现。乐观锁的工作原理：读取出数据时，将此版本号一同读出，之后更新时，对此版本号加一。此时，将提交数据的版本数据与数据库表对应记录的当前版本信息进行比对，如果提交的数据版本号大于数据库表当前版本号，则予以更新，否则认为是过期数据。
