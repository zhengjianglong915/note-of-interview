## 集合框架
Collection：List列表，Set集
Map：Hashtable，HashMap，TreeMap

Collection  是单列集合
List   元素是有序的(元素存取是有序)、可重复
有序的 collection，可以对列表中每个元素的插入位置进行精确地控制。可以根据元素的整数索引（在列表中的位置）访问元素，并搜索列表中的元素。可存放重复元素，元素存取是有序的。
List接口中常用类：
Vector：线程安全，但速度慢，已被ArrayList替代。底层数据结构是**数组结构**
ArrayList：线程不安全，查询速度快。底层数据结构是数组结构
LinkedList：线程不安全。增删速度快。底层数据结构是链表结构

Set(集) 元素不可重复。
取出元素的方法只有迭代器。**不可以存放重复元素，元素存取是无序的**。
Set接口中常用的类
HashSet：线程不安全，内部是基于散列函数实现,存取速度快。它是如何保证元素唯一性的呢？依赖的是元素的hashCode方法和euqals方法。.
  LinkedHashSet:具有HashSet的查询速度，且内部采用双向链表**维护元素的顺序**（默认保持插入顺序排序， 如果初始化时将accessOrder设置为true将使用最近最少顺序，即最近使用的元素插在链表尾部）。元素必须实现hashCode(）方法。内部是由链表实现。
TreeSet：线程不安全，可以对Set集合中的元素进行排序,底层采用了红黑树。它的排序是如何进行的呢？通过compareTo或者compare方法中的来保证元素的唯一性。元素是以二叉树的形式存放的。意味着TreeSet中的元素要实现Comparable接口。或者有一个自定义的比较器。

Map　是一个双列集合
|--Hashtable:线程安全，速度快。底层是**哈希表数据**结构。是同步的。不允许null作为键，null作为值。
   |--Properties:用于配置文件的定义和操作，使用频率非常高，同时键和值都是字符串。是集合中可以和IO技术相结合的对象。(到了IO在学习它的特有和io相关的功能。)
|--HashMap:线程不安全，速度慢。底层也是哈希表数据结构。是不同步的。
允许null作为键，null作为值。替代了Hashtable.
   |--LinkedHashMap: 可以保证HashMap集合有序。存入的顺序和取出的顺序一致。
|--TreeMap：可以用来对Map集合中的键进行排序.底层是采用红黑树．
Collection 和 Collections的区别
Collection是集合类的上级接口，子接口主要有Set 和List、Map。 
Collections是针对集合类的一个帮助类，提供了操作集合的工具方法：一系列静态方法实现对各种集合的搜索、排序、线程安全化等操作。 

## 接口：Collection
Collection是最基本的集合接口，**一个Collection代表一组Object，即Collection的元素（Elements）**。一些Collection允许相同的元素而另一些不行。一些能排序而另一些不行。Java SDK不提供直接继承自Collection的类，Java SDK提供的类都是继承自Collection的“子接口”如List和Set。
所有实现Collection接口的类都必须提供**两个标准的构造函数**：无参数的构造函数用于创建一个空的Collection，有一个Collection参数的构造函数用于创建一个新的Collection，这个新的Collection与传入的Collection有相同的元素。后一个构造函数允许用户复制一个Collection。
主要的一个接口方法：boolean add(Ojbect c)虽然返回的是boolean，但不是表示添加成功与否，这个返回值表示的意义是add()执行后，集合的内容是否改变了（就是元素的数量、位置等有无变化）。类似的addAll，remove，removeAll，remainAll也是一样的。
用Iterator模式实现遍历集合,Collection**有一个重要的方法：iterator()，返回一个Iterator（迭代器），用于遍历集合的所有元素**。Iterator模式可以把访问逻辑从不同的集合类中抽象出来，从而避免向客户端暴露集合的内部结构。典型的用法如下：

```
		Iterator it = collection.iterator(); // 获得一个迭代器
		while(it.hasNext()) {
			Object obj = it.next(); // 得到下一个元素
	}
```

不需要维护遍历集合的“指针”，所有的内部状态都由Iterator来维护，而这个Iterator由集合类通过工厂方法生成。每一种集合类返回的Iterator具体类型可能不同，但它们都实现了Iterator接口，因此，我们不需要关心到底是哪种Iterator，它只需要获得这个Iterator接口即可，这就是接口的好处，面向对象的威力。
要确保遍历过程顺利完成，必须保证遍历过程中不更改集合的内容（Iterator的remove()方法除外），所以，确保遍历可靠的原则是：只在一个线程中使用这个集合，或者在多线程中对遍历代码进行同步。由Collection接口派生的两个接口是List和Set。

## Collection包结构，与Collections的区别
Collection是集类，包含List有序列表，Set无序集合以及Map双列集合 Collection是集合类的上级接口，子接口主要有Set 和List、Map。  
Collections是针对集合类的一个**帮助类**，提供了操作集合的工具方法：一系列静态方法实现对各种集合的搜索、排序、线程安全化等操作

## List、Map、Set 三个接口存储元素时各有什么特点
1）**List**是有序的Collection，使用此接口**能够精确的控制每个元素插入的位置**。用户能够使用索引（元素在List中的位置，类似于数组下标）来访问List中的元素，这类似于Java的数组。和下面要提到的Set不同，List**允许有相同**的元素。
除了具有Collection接口必备的iterator()方法外，List还提供一个listIterator()方法，返回一个ListIterator接口，和标准的Iterator接口相比，ListIterator多了一些add()之类的方法，允许添加，删除，设定元素，还能向前或向后遍历。实现List接口的常用类有LinkedList，ArrayList，Vector和Stack。
2）**Set** 是一种不包含重复的元素的 Collection，即任意的两个元素 e1 和 e2 都有e1.equals(e2)=false，**Set 最多有一个 null 元素**。
3）**Map** 接口 ：请注意，Map 没有继承 Collection 接口，Map 提供 key 到 value 的映射．使用底层散列函数

## Map、Set、List、Queue、Stack的特点与用法
Set集合类似于一个罐子，"丢进"Set集合里的多个对象之间没有明显的顺序。 
List集合代表元素有序、可重复的集合，集合中每个元素都有其对应的顺序索引。 
Stack是Vector提供的一个子类，用于模拟"栈"这种数据结构(LIFO后进先出)
Queue用于模拟"队列"这种数据结构(先进先出 FIFO)。 
Map用于保存具有**"映射关系"**的数据，因此Map集合里保存着两组值

## Array和ArrayList有何区别？什么时候更适合用Array
Array可以容纳**基本类型和对象**，而**ArrayList只能容纳对象**。
Array是**指定大小的**，而ArrayList大小是固定的。
**Array没有提供ArrayList那么多功能**，比如addAll、removeAll和iterator等。尽管ArrayList明显是更好的选择，但也有些时候Array比较好用。
（1）如果列表的大小已经指定，大部分情况下是存储和遍历它们。
（2）对于遍历**基本数据类型**，尽管Collections使用自动装箱来减轻编码任务，在指定大小的基本类型的列表上工作也会变得很慢。
（3）如果你要使用**多维数组**，使用[][]比List更容易。

## ArrayList 和 LinkedList 
ArrayList 和 LinkedList 都实现了 List 接口，他们有以下的不同点：
ArrayList 是基于索引的数据接口，它的底层是数组。它可以以O(1)时间复杂度对元素进行**随机访问**。与此对应，LinkedList 是以元素列表的形式存储它的数据，每一个元素都和它的前一个和后一个元素链接在一起，在这种情况下，查找某个元素的时间复杂度是O(n)。
相对于 ArrayList，LinkedList的**插入，添加，删除操作速度更快**，因为当元素被添加到集合任意位置的时候，不需要像数组那样重新计算大小或者是更新索引。
LinkedList 比 ArrayList **更占内存**，因为 LinkedList 为每一个节点存储了两个引用，一个指向前一个元素，一个指向下一个元素。

## ArrayList和Vector异同点
 ArrayList和Vector在很多时候都很类似。
（1）两者都是基于索引的，内部由一个**数组支持**。
（2）两者维护插入的顺序，我们可以**根据插入顺序来获取元素**。
（3）ArrayList和Vector的迭代器实现都是**fail-fast**的。
（4）ArrayList和Vector两者**允许null值**，也可以使用索引值对元素进行随机访问。

以下是ArrayList和Vector的不同点。
（1）Vector是同步的，而ArrayList不是。然而，如果你寻求在迭代的时候对列表进行改变，你应该使用CopyOnWriteArrayList。
（2）ArrayList比Vector快，它因为有同步，不会过载。
（3）ArrayList更加通用，因为我们可以使用Collections工具类轻易地获取同步列表和只读列表。

## 遍历一个List有哪些不同的方式

		List<String> strList = new ArrayList<>();
		//使用for-each循环
		for(String obj : strList){
		  System.out.println(obj);
		}
		//using iterator
		Iterator<String> it = strList.iterator();
		while(it.hasNext()){
			String obj = it.next();
			System.out.println(obj);
		}

使用**迭代器更加线程安全，因为它可以确保，在当前遍历的集合元素被更改的时候，它会抛出ConcurrentModificationException**。

## LinkedList 和PriorityQueue 的区别
**Queue中只有两个实现类LinkedList 和PriorityQueue**。他们都是属于队列，即拥有先进先出（FIFO）的特点。而这两个类的区别在于他们的排序行为并非性能。
LinkedList 支持双向列表操作，
PriorityQueue 按优先权控制队列元素的出队次序。（列表中的排序顺序是通过实现Comparable来控制的）

## HashMap的工作原理
HashMap在Map.Entry静态内部类实现中存储key-value对。HashMap使用哈希算法，在put和get方法中，它使用hashCode()和equals()方法。当我们通过传递key-value对调用put方法的时候，HashMap使用Key hashCode()和哈希算法来找出存储key-value对的索引。Entry存储在LinkedList中，所以如果存在entry，它使用equals()方法来检查传递的key是否已经存在，如果存在，它会覆盖value，如果不存在，它会创建一个新的entry然后保存。当我们通过传递key调用get方法时，它再次使用hashCode()来找到数组中的索引，然后使用equals()方法找出正确的Entry，然后返回它的值。

其它关于HashMap比较重要的问题**是容量、加载因子和阀值调整**。HashMap默认的初始容量是16，**负荷系数是0.75**。阀值是为负荷系数乘以容量，无论何时我们尝试添加一个entry，如果map的大小比阀值大的时候，HashMap会对map的内容进行重新哈希，且使用更大的容量。**容量总是2的幂**，所以如果你知道你需要存储大量的key-value对，比如缓存从数据库里面拉取的数据，使用正确的容量和负荷系数对HashMap进行初始化是个不错的做法。

## HashMap实现原理
**数据结构**： HashMap基于哈希算法，采用链地址法来避免hash冲突，所以其内部采用链表数组数据结构。我们通过put()和get()方法储存和获取对象。HashMap里面实现一个静态内部类Entry, Entry其重要的属性有key , value, next.用该Entry定义了一个链表的数组用来存放数据，这种方法叫做连地址法，是为了避免hash冲突。
HashMap的存取实现:
1) put
首先会根据对象的hashCode计算得到**散列桶坐标**（数组下表），因为数组中存放的是**Entry链表结构**，所以还需要对该链表进行遍历，通过调用key.equals方法判断是否存在该key，如果存在则修改内容。如果不存在则**采用头插入**的方式在链表头部插入一个新的节点存储这个新的映射关系。
**null key总是存放在Entry[]数组的第一个元素，而且仅保留一个null key**。
2) get
首先也会根据hashCode的值定位到散列桶，并遍历存放在改桶中的链表。如果存在的该key则返回对应的value，**如果不存在则返回null**。（比较时调用key.equals)  具体比较：
(e.hash == hash && ((k = e.key) == key || key.equals(k)))）
再散列rehash过程：
Entry[]的长度一定后，随着map里面数据的越来越长，超过了加载因子，必须调整table的大小。每次将数组拓展到原来数据长度的两倍。这时，需要创建一张新表，将原表的映射到新表中。
注：**在存储当前值后再判断是否进行扩展容器**，这样的扩容方式有可能导致拓展后容器没有被使用造成浪费。
解决hash冲突的办法
1.开放定址法（线性探测再散列，二次探测再散列，伪随机探测再散列）
2.再哈希法
3.链地址法
4.建立一个公共溢出区
Java中hashmap的解决办法就是采用的链地址法。
HashMap实现原理：http://blog.csdn.net/vking_wang/article/details/14166593

## 我们能否使用任何类作为Map的key
 我们可以使用任何类作为Map的key，然而在使用它们之前，需要考虑以下几点：
（1）**如果类重写了equals()方法，它也应该重写hashCode()方法**。
（2）**类的所有实例需要遵循与equals()和hashCode()相关的规则**。请参考之前提到的这些规则。
（3）如果一个类没有使用equals()，你不应该在hashCode()中使用它。
（4）**用户自定义key类的最佳实践是使之为不可变的**，这样，hashCode()值可以被缓存起来，拥有更好的性能。不可变的类也可以确保hashCode()和equals()在未来不会改变，这样就会解决与可变相关的问题了。
    比如，我有一个类MyKey，在HashMap中使用它

		//传递给MyKey的name参数被用于equals()和hashCode()中
		MyKey key = new MyKey('Pankaj'); //assume hashCode=1234
		myHashMap.put(key, 'Value');
		// 以下的代码会改变key的hashCode()和equals()值
		key.setName('Amit'); //assume new hashCode=7890
		//下面会返回null，因为HashMap会尝试查找存储同样索引的key，而key已被改变了，匹配失败，返回null
		myHashMap.get(new MyKey('Pankaj'));

那就是为何String和Integer被作为HashMap的key大量使用

## 如何决定选用HashMap还是TreeMap
对于在**Map中插入、删除和定位元素这类操作，HashMap是最好的选择**。然而，**假如你需要对一个有序的key集合进行遍历，TreeMap是更好的选择**。基于你的collection的大小，也许向HashMap中添加元素会更快，将map换为TreeMap进行有序key的遍历。

## HashMap和HashTable的区别
HashMap和Hashtable都实现了Map接口，因此很多特性非常相似。但是，他们有以下不同点：
1)HashMap允许键和值是null，而Hashtable不允许键或者值是null。
2)Hashtable是同步的，而HashMap不是。因此，HashMap更适合于单线程环境，而Hashtable适合于多线程环境。
3）HashMap的**迭代器(Iterator)是fail-fast迭代器**，而Hashtable的**enumerator(列举)迭代器不是fail-fast的**。所以当有其它线程改变了HashMap的结构（增加或者移除元素），将会抛出ConcurrentModificationException，但迭代器本身的remove()方法移除元素则不会抛出ConcurrentModificationException异常。但这并不是一个一定发生的行为，要看JVM。这条同样也是Enumeration和Iterator的区别。一般认为Hashtable是一个遗留的类。如果你寻求在迭代的时候修改Map，你应该使用CocurrentHashMap
http://www.importnew.com/7010.html

## HashMap 和 Hashtable 有什么区别
HashMap 和 Hashtable 都实现了 Map 接口，因此很多特性非常相似。但是，他们有以下不同点： 
HashMap 允许键和值是 null，而 Hashtable 不允许键或者值是 null。
Hashtable 是同步的，而 HashMap 不是。因此， HashMap 更适合于单线程环境，而 Hashtable 适合于多线程环境。
HashMap 提供了可供应用迭代的键的集合，因此，HashMap 是快速失败的。另一方面，Hashtable 提供了对键的列举(Enumeration)。

## Enumeration 接口和 Iterator 接口的区别
Enumeration 速度是 Iterator 的2倍，**同时占用更少的内存**。
但是，Iterator远远比 Enumeration 安全，因为其他线程不能够修改正在被 iterator 遍历的集合里面的对象。
同时，Iterator 允许调用者删除底层集合里面的元素，这对 Enumeration 来说是不可能的。

## 通过迭代器fail-fast属性
每次我们尝试获取下一个元素的时候，Iterator fail-fast属性检查当前集合结构里的任何改动。如果发现任何改动，它抛出ConcurrentModificationException。Collection中所有Iterator的实现都是按fail-fast来设计。在遍历一个集合的时候，我们可以使用并发集合类来避免ConcurrentModificationException，比如使用CopyOnWriteArrayList，而不是ArrayList

## 快速失败(fail-fast)和安全失败(fail-safe)的区别
Iterator的fail-fast属性与当前的集合共同起作用，因此它不会受到集合中任何改动的影响。java.util包下面的所有的集合类都是快速失败的，而java.util.concurrent包下面的所有的类都是安全失败的。快速失败的迭代器会抛出ConcurrentModificationException异常，而安全失败的迭代器永远不会抛出这样的异常。

## HashSet,TreeSet,LinkedHashSet 之间的区别
首先这三个类都是实现了Set接口，并且LinkedHashSet继承HashSet ，TreeSet实现了SortedSet。所以它们都拥有Set的基本特性，如：集合中的元素不能重复（这个需要和容器转载的类中的**equals()方法**有关）. 这三个容器所接受的类必须实现equals() 方法。
**HashSet**： 为查询速度所设计的，存入HashSet的元素必须定义hashCode()方法。内部是基于散列函数实现。
**TreeSet**： 实现了SortedSet（SortedSet的元素可以保证处于排序状态）,所以内部的元素是保持一定次序的（次序与元素实现的compareTo()方法有关），且底层是树状结构。使用它可以从Set中提取有序的序列。要求存入的元素必须实现Comparable接口,或则需要传入一个比较器Comparator。否则报ClassCastException。内部采用了**红黑树**实现。
**LinkedHashSet**： 具有HashSet的查询速度，且内部链表维护元素的顺序（按插入顺序排序）。元素必须实现hashCode(）方法。内部是由链表实现。

## HashMap ,LinkedHashMap ,TreeMap,WeakHashMap,ConcurrentHashMap,IdentityHashMap的区别
**Map** 中任何键的比较都是通过equals进行比较的，所以如果键用于映射的话需要由恰当的hashCode() 方法。如果键值用于TreeMap，它必须实现Comparable。
**HashMap** 基于散列表来的实现，即使用hashCode()进行快速查询元素的位置，显著提高性能。插入和查询“键值对”的开销是固定的。可以通过设置容量和负载因子，以调整容器的性能。
**LinkedHashMap**,  类似于HashMap,但是迭代遍历它时，取得“键值对”的顺序是其插入的次序，只比HashMap慢一点。而在迭代访问时反而更快，**因为它使用链表维护内部次序**。此外可以在构造器中设定LinkedHashMap，使之采用基于访问的最近最少使用(LRU)算法。于是没有被访问过的元素就会出现在队列前面。对于需要定期清理元素以节省空间的程序员来说，此功能使得程序员很容易得以实现。被访问过的键值会放在链表尾部。
**TreeMap**, 是基于**红黑树**的实现。实现了SortedMap，SortedMap 可以确保键处于排序状态。所以查看“键”和“键值对”时，所有得到的结果都是经过排序的，次序由Comparable或Comparator决定。SortedMap拥有其他额外的功能，如：Comparator comparator()返回当前Map使用的Comparator或者null. T firstKey() 返回Map的第一个键，T lastKey() 返回最后一个键。SortedMap headMap(toKey),生成一个键小于toKey的Map子集。SortedMap tailMap(fromKey) 也是生成一个子集。TreeMap是唯一的带有subMap()方法的Map,它可以返回一个子树。
**WeakHashMap**  表示**弱键映射**，允许释放映射所指向的对象。这是为了解决某类特殊问题而设计的，如果映射之外没有引用指向某个“键”，则“键”可以被垃圾收集器回收。
**ConcurrentHashMap** 一种线程安全的Map,它不涉及同步加锁。
**IdentityHashMap** 使用==代替equals() 对“键”进行比较的散列映射。专为解决特殊问题而设计。
**HashTable** (已经摒弃了)

## Map接口提供了哪些不同的集合视图
Map接口提供三个集合视图：
（1）**Set keyset()**：**返回map中包含的所有key的一个Set视图**。**集合是受map支持的，map的变化会在集合中反映出来，反之亦然**。当一个迭代器正在遍历一个集合时，若map被修改了（除迭代器自身的移除操作以外），迭代器的结果会变为未定义。集合支持通过**Iterator**的Remove、Set.remove、removeAll、retainAll和clear操作进行元素移除，从map中移除对应的映射。它不支持add和addAll操作。
（2）**Collection values()**：**返回一个map中包含的所有value的一个Collection视图**。这个collection受map支持的，map的变化会在collection中反映出来，反之亦然。当一个迭代器正在遍历一个collection时，若map被修改了（除迭代器自身的移除操作以外），迭代器的结果会变为**未定义**。集合支持通过Iterator的Remove、Set.remove、removeAll、retainAll和clear操作进行元素移除，从map中移除对应的映射。它不支持add和addAll操作。
（3）Set< Map.Entry < K,V > > entrySet()：**返回一个map钟包含的所有映射的一个集合视图**。这个集合受map支持的，map的变化会在collection中反映出来，反之亦然。当一个迭代器正在遍历一个集合时，若map被修改了（除迭代器自身的移除操作，以及对迭代器返回的entry进行setValue外），**迭代器的结果会变为未定义**。集合支持通过Iterator的Remove、Set.remove、removeAll、retainAll和clear操作进行元素移除，从map中移除对应的映射。它不支持add和addAll操作。


## hashCode()和equals()方法有何重要性
HashMap使用Key对象的hashCode()和equals()方法去决定key-value对的索引。当我们试着从HashMap中获取值的时候，这些方法也会被用到。如果这些方法没有被正确地实现，在这种情况下，两个不同Key也许会产生相同的hashCode()和equals()输出，HashMap将会认为它们是相同的，然后覆盖它们，而非把它们存储到不同的地方。同样的，所有不允许存储重复数据的集合类都使用hashCode()和equals()去查找重复，所以正确实现它们非常重要。equals()和hashCode()的实现应该遵循以下规则：
（1）如果o1.equals(o2)，那么o1.hashCode() == o2.hashCode()总是为true的。
（2）如果o1.hashCode() == o2.hashCode()，并不意味着o1.equals(o2)会为true。

## Iterator是什么
Iterator接口提供遍历任何Collection的接口。我们可以从一个Collection中使用迭代器方法来获取迭代器实例。迭代器取代了Java集合框架中的Enumeration。迭代器允许调用者在迭代过程中移除元素。

## Iterator和ListIterator的区别是什么
下面列出了他们的区别：
Iterator 可用来遍历 Set 和 List 集合，但是 ListIterator 只能用来遍历 List 。
Iterator 对集合只能是前向遍历，ListIterator 既可以前向也可以后向。
ListIterator 实现了 Iterator 接口，并包含其他的功能，比如：增加元素，替换元素，获取前一个和后一个元素的索引，等等。 

## 为何Iterator接口没有具体的实现
Iterator接口定义了遍历集合的方法，但它的实现则是集合实现类的责任。每个能够返回用于遍历的Iterator的集合类都有它自己的Iterator实现内部类。
这就允许集合类去选择迭代器是fail-fast还是fail-safe的。比如，ArrayList迭代器是fail-fast的，而CopyOnWriteArrayList迭代器是fail-safe的

## EnumSet是什么
java.util.EnumSet是使用**枚举类型**的集合实现。当集合创建时，枚举集合中的所有元素必须来自单个指定的枚举类型，可以是显示的或隐示的。EnumSet是不同步的，不允许值为null的元素。它也提供了一些有用的方法，比如copyOf(Collection c)、of(E first,E…rest)和complementOf(EnumSet s)。

## 哪些集合类是线程安全的
Vector、HashTable、Properties和Stack是同步类，所以它们是线程安全的，可以在多线程环境下使用。Java1.5并发API包括一些集合类，允许迭代时修改，因为它们都工作在集合的克隆上，所以它们在多线程环境中是安全的。

## BockingQueue是什么
Java.util.concurrent.BlockingQueue是一个队列，在进行检索或移除一个元素的时候，它会等待队列变为非空；当在添加一个元素时，它会等待队列中的可用空间。BlockingQueue接口是Java集合框架的一部分，主要用于实现生产者-消费者模式。我们不需要担心等待生产者有可用的空间，或消费者有可用的对象，因为它都在BlockingQueue的实现类中被处理了。Java提供了集中BlockingQueue的实现，比如ArrayBlockingQueue、LinkedBlockingQueue、PriorityBlockingQueue,、SynchronousQueue等。

## 我们如何对一组对象进行排序
 如果我们需要对一个对象数组进行排序，我们可以使用Arrays.sort()方法。如果我们需要排序一个对象列表，我们可以使用Collections.sort()方法。两个类都有用于自然排序（使用Comparable）或基于标准的排序（使用Comparator）的重载方法sort()。Collections内部使用数组排序方法，所有它们两者都有相同的性能，只是Collections需要花时间将列表转换为数组。

## Comparable和Comparator接口
Comparable & Comparator 都是用来实现集合中元素的比较、排序的，只是 Comparable 是在集合内部定义的方法实现的排序，Comparator 是在集合外部实现的排序，所以，如想实现排序，就需要在集合外定义 Comparator 接口的方法或在集合内实现 Comparable 接口的方法。
1) Comparator位于包java.util下，而Comparable位于包java.lang下
2)  **Comparable 是在集合内部定义的方法实现的排序,Comparator 是在集合外部实现的排序**.Comparable 是一个对象本身就已经支持自比较所需要实现的接口,自定义的类要在加入list容器中后能够排序，可以实现Comparable接口.在用Collections类的sort方法排序时，如果不指定Comparator，那么就以自然顺序排序.而 Comparator 是一个专用的比较器，当这个对象不支持自比较或者自比较函数不能满足你的要求时，你可以写一个比较器来完成两个对象之间大小的比较。**用 Comparator 是策略模式（strategy design pattern），就是不改变对象自身，而用一个策略对象（strategy object）来改变它的行为**。
3) Comparator定义了俩个方法，分别是int compare(T o1,T o2)和boolean equals(Object obj).Comparable接口只提供了int compareTo(T o)方法.
也就是说假如我定义了一个Person类，这个类实现了Comparable接口，那么当我实例化Person类的person1后，我想比较person1和一个现有的Person对象person2的大小时，我就可以这样来调用：person1.comparTo(person2),通过返回值就可以判断了；而此时如果你定义了一个PersonComparator（实现了Comparator接口）的话，那你就可以这样：PersonComparator   comparator=   new   PersonComparator();
comparator.compare(person1,person2);。

## Java集合框架是什么？说出一些集合框架的优点？
每种编程语言中都有集合，最初的Java版本包含几种集合类：Vector、Stack、HashTable和Array。随着集合的广泛使用，Java1.2提出了囊括所有集合接口、实现和算法的集合框架。在保证线程安全的情况下使用泛型和并发集合类，Java已经经历了很久。它还包括在Java并发包中，阻塞接口以及它们的实现。集合框架的部分优点如下：
（1）使用核心集合类降低开发成本，而非实现我们自己的集合类。
（2）随着使用经过严格测试的集合框架类，代码质量会得到提高。
（3）通过使用JDK附带的集合类，可以降低代码维护成本。
（4）复用性和可操作性。

## 集合框架中的泛型有什么优点
Java1.5引入了泛型，所有的集合接口和实现都大量地使用它。泛型允许我们为集合提供一个可以容纳的对象类型，因此，如果你添加其它类型的任何元素，它会在编译时报错。这避免了在运行时出现ClassCastException，因为你将会在编译时得到报错信息。泛型也使得代码整洁，我们不需要使用显式转换和instanceOf操作符。它也给运行时带来好处，因为不会产生类型检查的字节码指令。

## 为何Map接口不继承Collection接口
尽管Map接口和它的实现也是集合框架的一部分，但Map不是集合，集合也不是Map。因此，Map继承Collection毫无意义，反之亦然。
如果Map继承Collection接口，那么元素去哪儿？Map包含key-value对，它提供抽取key或value列表集合的方法，但是它不适合“一组对象”规范。

## 为什么集合类没有实现Cloneable和Serializable接口
Collection接口指定一组对象，对象即为它的元素。**如何维护这些元素由Collection的具体实现决定**。例如，一些如List的Collection实现允许重复的元素，而其它的如Set就不允许。**很多Collection实现有一个公有的clone方法。然而，把它放到集合的所有实现中也是没有意义的。这是因为Collection是一个抽象表现。重要的是实现**。
**当与具体实现打交道的时候，克隆或序列化的语义和含义才发挥作用**。所以，具体实现应该决定如何对它进行克隆或序列化，或它是否可以被克隆或序列化。
**在所有的实现中授权克隆和序列化，最终导致更少的灵活性和更多的限制**。特定的实现应该决定它是否可以被克隆和序列化。

## 与Java集合框架相关的有哪些最好的实践
(1)根据需要选择正确的集合类型。比如，如果指定了大小，我们会选用Array而非ArrayList。如果我们想根据插入顺序遍历一个Map，我们需要使用TreeMap。如果我们不想重复，我们应该使用Set。如果涉及到堆栈，队列等操作，应该考虑用List，对于需要快速插入，删除元素，应该使用LinkedList，如果需要快速随机访问元素，应该使用ArrayList。
(2)如果程序在单线程环境中，或者访问仅仅在一个线程中进行，考虑非同步的类，其效率较高，如果多个线程可能同时操作一个类，应该使用同步的类。
(3)要特别注意对哈希表的操作，作为key的对象要正确复写equals和hashCode方法。使用JDK提供的不可变类作为Map的key，可以避免自己实现hashCode()和equals()。
性。
(4)尽量返回接口而非实际的类型，如返回List而非ArrayList，这样如果以后需要将ArrayList换成LinkedList时，客户端代码不用改变。这就是针对抽象编程。
(5)尽可能使用Collections工具类，或者获取只读、同步或空的集合，而非编写自己的实现。它将会提供代码重用性，它有着更好的稳定性和可维护
(6)一些集合类允许指定**初始容量**，所以如果我们能够估计到存储元素的数量，我们可以使用它，就避免了重新哈希或大小调整。
(7)基于接口编程，而非基于实现编程，它允许我们后来轻易地改变实现。
(8)总是使用类型安全的泛型，避免在运行时出现ClassCastException。

## ArrayList 是否支持序列化
支持，
实现了　serlializable接口，并实现了以下两个方法：
```
　　 　private void writeObject(java.io.ObjectOutputStream s)
	throws java.io.IOException{
	// Write out element count, and any hidden stuff
	int expectedModCount = modCount;
	s.defaultWriteObject();

	// Write out size as capacity for behavioural compatibility with clone()
	s.writeInt(size);

	// Write out all elements in the proper order.
	for (int i=0; i<size; i++) {
	    s.writeObject(elementData[i]);
	}

	if (modCount != expectedModCount) {
	    throw new ConcurrentModificationException();
	}
    }

    /**
     * Reconstitute the <tt>ArrayList</tt> instance from a stream (that is,
     * deserialize it).
     */
    private void readObject(java.io.ObjectInputStream s)
	throws java.io.IOException, ClassNotFoundException {
	elementData = EMPTY_ELEMENTDATA;

	// Read in size, and any hidden stuff
	s.defaultReadObject();

	// Read in capacity
	s.readInt(); // ignored

	if (size > 0) {
	    // be like clone(), allocate array based upon size not capacity
	    ensureCapacityInternal(size);

	    Object[] a = elementData;
	    // Read in all elements in the proper order.
	    for (int i=0; i<size; i++) {
	        a[i] = s.readObject();
	    }
	}
    }
```

ArrayList
```
 public Object clone() {
	try {
	    ArrayList<?> v = (ArrayList<?>) super.clone();
	    v.elementData = Arrays.copyOf(elementData, size);
	    v.modCount = 0;
	    return v;
	} catch (CloneNotSupportedException e) {
	    // this shouldn't happen, since we are Cloneable
	    throw new InternalError(e);
	}
    }
```
## 为什么集合类没有实现 Cloneable 和 Serializable 接口
## Comparable 和Comparator 接口


个人公众号（欢迎关注）：
![](/assets/weix_gongzhonghao.jpg)


