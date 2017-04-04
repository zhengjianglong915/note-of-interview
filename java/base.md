## 1.Java 语言的优点
面向对象,平台无关,内存管理,安全性,多线程,Java 是解释型的

### 2.Java 和 C++的区别
1. **多重继承**(java接口多重,类不支持,C++支持)
2. **自动内存管理**
3. **预处理功能**
4. **goto语句**(java不支持)
5. **引用与指针**。在Java中不可能直接操作对象本身，所有的对象都由一个引用指向，必须通过这个引用才能访问对象本身，包括获取成员变量的值，改变对象的成员变量，调用对象的方法等。而在C++中存在引用，对象和指针三个东西，这三个东西都可以访问对象。其实，Java中的引用和C++中的指针在概念上是相似的，他们都是存放的对象在内存中的地址值，只是在Java中，引用丧失了部分灵活性，比如Java中的引用不能像C++中的指针那样进行**加减运算**。 

## 3.值传递和引用传递
变量被值传递，意味着传递了**变量的一个副本**。因此，就算是改变了变量副本，也不会影响源对象的值。
对象被引用传递，意味着传递的并不是实际的对象，而是**对象的引用**。因此，外部对引用对象所做的改变会反映到所有的对象上。
java本质上还是值传递，如方法调用的时候传入一个对象引用进去，在方法栈中会构建一个副本和该引用变量值相同指向同一个地址。如果改变引用的值不会对改变传入的引用的值。

## 4.静态变量和实例变量的区别
**在语法定义上的区别**：静态变量前要加 static 关键字，而实例变量前则不加。
**在程序运行时的区别**：实例变量属于某个对象的属性，必须创建了实例对象，其中的实例变量才会被分配空间，才能使用这个实例变量。静态变量不属于某个实例对象，而是属于类，所以也称为类变量，只要程序加载了类的字节码，不用创建任何实例对象，静态变量就会被分配空间，静态变量就可以被使用了。总之，实例变量必须创建对象后才可以通过这个对象来使用，静态变量则可以直接使用类名来引用。

## 5.JDK 包.
JDK 常用的 package
**java.lang**：这个是系统的基础类，比如 String 等都是这里面的，这个 package 是唯一一个可以不用 import 就可以使用的 Package
**java.io**: 这里面是所有输入输出有关的类，比如文件操作等
**java.net**: 这里面是与网络有关的类，比如 URL,URLConnection 等。
**java.util**: 这个是系统辅助类，特别是集合类 Collection,List,Map 等。
**java.sql**: 这个是数据库操作的类，Connection, Statememt，ResultSet 等

## 6.JDK, JRE 和 JVM 的区别
JDK,JRE和JVM 是 Java 编程语言的核心概念。尽管它们看起来差不多，作为程序员我们也不怎么关心这些概念，但是它们是不同的针对特定目的的产品。这是一道常见的 Java 面试题，而本文则会一一解释这些概念并给出它们之间的区别。
**1）Java 开发工具包 (JDK)**
Java 开发工具包是 Java 环境的**核心组件，并提供编译、调试和运行一个 Java 程序所需的所有工具，可执行文件和二进制文件**。包括java基础jar包、虚拟机、javac等可执行文件等。JDK 是一个平台特定的软件，有针对 Windows，Mac 和 Unix 系统的不同的安装包。可以说 JDK 是 JRE 的超集，它包含了 JRE 的 Java 编译器，调试器和核心类。
**2）Java 虚拟机(JVM)**
JVM 是 Java 编程语言的核心。当我们运行一个程序时，JVM 负责将字节码转换为特定机器代码。JVM 也是平台特定的，并提供核心的 Java 方法，例如内存管理、垃圾回收和安全机制等。JVM 是可定制化的，我们可以通过 Java 选项(java options)定制它，比如配置 JVM 内存的上下界。JVM 之所以被称为虚拟的是因为它提供了一个不依赖于底层操作系统和机器硬件的接口。这种独立于硬件和操作系统的特性正是 Java 程序可以一次编写多处执行的原因。
**3）Java 运行时环境(JRE)**
JRE 是 JVM 的实施实现，它提供了运行 Java 程序的平台。**JRE 包含了 JVM、Java 二进制文件和其它成功执行程序的类文件**。JRE 不包含任何像 Java 编译器、调试器之类的开发工具。如果你只是想要执行 Java 程序，你只需安装 JRE 即可，没有安装 JDK 的必要。

**JDK, JRE 和 JVM 的区别**
JDK 是用于开发的而 JRE 是用于运行 Java 程序的。
JDK 和 JRE 都包含了 JVM，从而使得我们可以运行 Java 程序。
JVM 是 Java 编程语言的核心并且具有平台独立性。
**即时编译器(JIT)**
有时我们会听到 JIT 这个概念，并说它是 JVM 的一部分，这让我们很困惑。JIT 是 JVM 的一部分，它可以在同一时间编译类似的字节码来优化将字节码转换为机器特定语言的过程相似的字节码，从而将优化字节码转换为机器特定语言的过程，这样减少转换过程所需要花费的时间。

## 7.是否可以在 static 环境中访问非 static 变量
static 变量在 Java 中是属于类的，它在所有的实例中的值是一样的。当类被Java虚拟机载入的时候，会对 static 变量进行初始化。如果你的代码尝试不用实例来访问非 static 的变量，编译器会报错，因为这些变量还没有被创建出来，还没有跟任何实例关联上。

## Java 中的 final关键字用法
(1)修饰类：表示该类不能被继承；
(2)修饰方法：表示方法不能被覆盖；
(3)修饰变量：表示变量只能一次赋值以后值不能被修改（常量）。

## assert
assertion(断言)在软件开发中是一种常用的调试方式，很多开发语言中都支持这种机制。一般来说，assertion **用于保证程序最基本、关键的正确性**。assertion 检查通常在开发和测试时开启。为了提高性能，在软件发布后， assertion 检查通常是关闭的。在实现中，断言是一个包含布尔表达式的语句，在执行这个语句时假定该表达式为 true；如果表达式计算为 false，那么系统会报告一个 AssertionError。

断言用于调试目的：

		assert(a > 0); // throws an AssertionError if a <= 0

断言可以有两种形式：

		assert Expression1;
		assert Expression1 : Expression2 ;

Expression1 应该总是产生一个布尔值。
Expression2 可以是得出一个值的任意表达式；这个值用于生成显示更多调试信息的**字符串消息**
断言在默认情况下是禁用的，要在编译时启用断言，需使用 source 1.4 标记：

		javac -source 1.4 Test.java

要在运行时启用断言，可使用 -enableassertions 或者 -ea 标记。
要在运行时选择禁用断言，可使用 -da 或者 -disableassertions 标记。

## final, finally, finalize 的区别?
**final**：修饰符（关键字）有三种用法：如果一个类被声明为final，意味着它不能再派生出新的子类，即不能被继承，因此它和 abstract 是反义词。将变量声明为 final，可以保证它们在使用中不被改变，被声明为 final 的变量必须在声明时给定初值，而在以后的引用中只能读取不可修改。被声明为 final 的方法也同样只能使用，不能在子类中被重写。
**finally**：通常放在 try…catch 的后面构造总是执行代码块，这就意味着程序无论正常执行还是发生异常，这里的代码只要 JVM 不关闭都能执行，可以将释放外部资源的代码写在 finally 块中。
**finalize**：Object 类中定义的方法，Java 中允许使用 finalize() 方法在垃圾收集器将对象从内存中清除出去之前做必要的清理工作。这个方法是由垃圾收集器在销毁对象时调用的，通过重写finalize() 方法可以整理系统资源或者执行其他清理工作。

## & 和 &&
& 和 && 都可以用作逻辑与的运算符，表示逻辑与（and），当运算符两边的表达式的结果都为 true 时，整个运算结果才为 true，否则，只要有一方为 false，则结果为 false。 
**&& 还具有短路的功能**，即如果第一个表达式为 false，则不再计算第二个表达式.& 还可以用作位运算符，当 & 操作符两边的表达式不是 boolean 类型时，& 表示按位与操作.

## 用最有效率的方法算出 2 乘以 8 等於几
2 << 3，因为将一个数左移 n 位，就相当于乘以了 2 的 n 次方，那么，一个数乘以 8 只要将其左移 3 位即可，而位运算 cpu 直接支持的，效率最高，所以，2 乘以 8 等於几的最效率的方法是 2 << 3 。

## char 型变量
char 类型可以存储一个中文汉字，因为 Java 中使用的编码是 Unicode（不选择任何特定的编码，直接使用字符在字符集中的编号，这是统一的唯一方法），一个 char 类型占 2 个字节（16bit），所以放一个中文是没问题的。
补充：使用 Unicode 意味着字符在 JVM 内部和外部有不同的表现形式，在 JVM 内部都是 Unicode，当这个字符被从 JVM 内部转移到外部时（例如存入文件系统中），需要进行编码转换。所以 Java 中有字节流和字符流，以及在字符流和字节流之间进行转换的转换流，如 InputStreamReader 和 OutputStreamReader.

## String 和StringBuilder、StringBuffer 的区别
答：Java 平台提供了两种类型的字符串：String 和StringBuffer / StringBuilder，它们可以储存和操作字符串。其中 String 是**只读**字符串，也就意味着 String 引用的字符串内容是不能被改变的。而 StringBuffer 和 StringBuilder 类表示的字符串对象可以直接进行修改。StringBuilder 是 JDK 1.5 中引入的，它和 StringBuffer 的方法完全相同，区别在于它是在单线程环境下使用的，因为它的所有方面都没有被 synchronized 修饰，因此它的效率也比 StringBuffer 略高。
有一个面试题问：有没有哪种情况用 + 做字符串连接比调用 StringBuffer / StringBuilder 对象的 append 方法性能更好？如果连接后得到的字符串在静态存储区中是早已存在的，那么用+做字符串连接是优于 StringBuffer / StringBuilder 的 append 方法的。

## String不可变性
至于为什么要把 String 类设计成不可变类，是它的用途决定的。其实不只 String，很多 Java 标准类库中的类都是不可变的。在开发一个系统的时候，我们有时候也需要设计不可变类，来传递一组相关的值，这也是面向对象思想的体现。不可变类有一些优点，比如因为它的对象是只读的，所以多线程并发访问也不会有任何问题。当然也有一些缺点，比如每个不同的状态都要一个对象来代表，可能会造成性能上的问题。所以 Java 标准类库还提供了一个可变版本，即 StringBuffer。
 Javac 编译可以对字符串常量直接相加的表达式进行优化，不必要等到运行期去进行加法运算处理，而是在**编译时去掉其中的加号**，直接将其编译成一个这些常量相连的结果。所以 
		String s=“a”+”b”+”c”+”d”;只生成一个对象.

## 不可变对象
如果一个对象，在它创建完成之后，不能再改变它的状态，那么这个对象就是不可变的。不能改变状态的意思是，不能改变对象内的成员变量，包括基本数据类型的值不能改变，引用类型的变量不能指向其他的对象，引用类型指向的对象的状态也不能改变。
**如何创建不可变类**
1. 将类声明为final，所以它不能被继承
2. 将所有的成员声明为私有的，这样就不允许直接访问这些成员
3. 对变量不要提供setter方法
4. 将所有可变的成员声明为final，这样只能对它们赋值一次
5. 通过构造器初始化所有成员，进行深拷贝(deep copy):如果某一个类成员不是原始变量(primitive)或者不可变类，必须通过在成员初始化(in)或者get方法(out)时通过深度clone方法，来确保类的不可变。
6. 在getter方法中，不要直接返回对象本身，而是克隆对象，并返回对象的拷贝

http://www.cnblogs.com/yg_zhang/p/4355354.html
http://www.importnew.com/7535.html
## 为什么String要设计成不可变的**
在Java中将String设计成不可变的是综合考虑到各种因素的结果,如内存,同步,数据结构以及安全等方面的考虑.
1. 字符串常量池的需要.
 字符串池的实现可以在运行时节约很多heap空间，因为不同的字符串变量都指向池中的同一个字符串。但如果字符串是可变的，那么String interning将不能实现(译者注：String interning是指对不同的字符串仅仅只保存一个，即不会保存多个相同的字符串。)，因为这样的话，如果变量改变了它的值，那么其它指向这个值的变量的值也会一起改变。
2. 线程安全考虑。
同一个字符串实例可以被多个线程共享。这样便不用因为线程安全问题而使用同步。字符串自己便是线程安全的。
3. 类加载器要用到字符串，不可变性提供了安全性，以便正确的类被加载。譬如你想加载java.sql.Connection类，而这个值被改成了myhacked.Connection，那么会对你的数据库造成不可知的破坏。
4. 支持hash映射和缓存。
因为字符串是不可变的，所以在它创建的时候hashcode就被缓存了，不需要重新计算。这就使得字符串很适合作为Map中的键，字符串的处理速度要快过其它的键对象。这就是HashMap中的键往往都使用字符串。

http://blog.csdn.net/renfufei/article/details/16808775
http://www.codeceo.com/article/why-java-string-immutable.html
http://www.importnew.com/7440.html
http://www.importnew.com/16817.html

## 什么是 Java 序列化，如何实现 Java 序列化
序列化就是一种用来处理对象流的机制，**所谓对象流也就是将对象的内容进行流化**。**可以对流化后的对象进行读写操作，也可将流化后的对象传输于网络之间**。序列化是为了解决在对对象流进行读写操作时所引发的问题。

序列化的实现：将需要被序列化的类实现 Serializable 接口，该接口没有需要实现的方法， implements Serializable 只是为了标注该对象是可被序列化的，然后使用一个输出流(如：FileOutputStream)来构造一个ObjectOutputStream(对象流)对象，接着，使用ObjectOutputStream对象的writeObject(Object obj)方法就可以将参数为obj的对象写出(即保存其状态)，要恢复的话则用输入流。




## 错误和异常的区别(Error vs Exception)
1) java.lang.Error: Throwable 的子类，**用于标记严重错误,表示系统级的错误和程序不必处理的异常**。合理的应用程序不应该去 try/catch 这种错误。是恢复不是不可能但很困难的情况下的一种严重问题；**比如内存溢出**，不可能指望程序能处理这样的情况；
java.lang.Exception: Throwable 的子类，**表示需要捕捉或者需要程序进行处理的异常，是一种设计或实现问题**；也就是说，它表示如果程序运行正常，从不会发生的情况。并且鼓励用户程序去 catch 它。

2) Error 和 RuntimeException 及其子类都是**未检查的异常（unchecked exceptions）**，而所有其他的 Exception 类都是检查了的异常（checked exceptions）
    **checked exceptions**: **上下文环境有关，即使程序设计无误，仍然可能因使用的问题而引发．通常是从一个可以恢复的程序中抛出来的，并且最好能够从这种异常中使用程序恢复**。比如 **FileNotFoundException**, ParseException 等。检查了的异常发生在编译阶段，必须要使用 try…catch（或者 throws ）否则编译不通过。
    **unchecked exceptions**:通常是如果一切正常的话本不该发生的异常，但是的确发生了。 **发生在运行期，具有不确定性，主要是由于程序的逻辑问题所引起的**。比如 ArrayIndexOutOfBoundException, ClassCastException 等。从语言本身的角度讲，程序不该去 catch 这类异常，虽然能够从诸如 RuntimeException 这样的异常中 catch 并恢复，但是并不鼓励终端程序员这么做，因为完全没要必要。因为这类错误本身就是 bug，应该被修复，出现此类错误时程序就应该立即停止执行。 因此，面对 Errors 和 unchecked exceptions 应该让程序自动终止执行，程序员不该做诸如 try/catch 这样的事情，而是应该查明原因，修改代码逻辑。
    RuntimeException：RuntimeException体系包括错误的类型转换、数组越界访问和试图访问空指针等等。处理 RuntimeException 的原则是：如果出现 RuntimeException，那么一定是程序员的错误。例如，可以通过检查数组下标和数组边界来避免数组越界访问异常。其他（IOException等等）checked 异常一般是外部错误，例如试图从文件尾后读取数据等，这并不是程序本身的错误，而是在应用环境中出现的外部错误。

## try{} 里的 return 语句
Java 允许在 finally 中改变返回值的做法是不好的，因为如果存在 finally 代码块，try 中的 return 语句不会立马返回调用者，而是记录下返回值待 finally 代码块执行完毕之后再向调用者返回其值，然后如果在 finally 中修改了返回值，这会对程序造成很大的困扰

## 运行时异常与受检异常有何异同？
答：异常表示程序运行过程中可能出现的非正常状态，运行时异常表示虚拟机的通常操作中可能遇到的异常，是一种常见运行错误，只要程序设计得没有问题通常就不会发生。受检异常跟程序运行的**上下文环境有关，即使程序设计无误，仍然可能因使用的问题而引发**。Java编译器要求方法必须声明抛出可能发生的受检异常，但是并不要求必须声明抛出未被捕获的运行时异常。异常和继承一样，是面向对象程序设计中经常被滥用的东西，神作《Effective Java》中对异常的使用给出了以下指导原则：
    **不要将异常处理用于正常的控制流（设计良好的API不应该强迫它的调用者为了正常的控制流而使用异常）**
    **对可以恢复的情况使用受检异常，对编程错误使用运行时异常**
    **避免不必要的使用受检异常（可以通过一些状态检测手段来避免异常的发生）**
    **优先使用标准的异常**
    **每个方法抛出的异常都要有文档**
    **保持异常的原子性**
    **不要在 catch 中忽略掉捕获到的异常**

## throws、throw、try、catch、finally 
一般情况下是用 try 来执行一段程序，如果出现异常，系统会抛出（throw）一个异常，这时候你可以通过它的类型来捕捉（catch）它，或最后（finally）由缺省处理器来处理；try 用来指定一块预防所有“异常”的程序；catch 子句紧跟在 try 块后面，用来指定你想要捕捉的“异常”的类型；throw 语句用来明确地抛出一个“异常”；throws用来标明一个成员函数可能抛出的各种“异常”；finally 为确保一段代码不管发生什么“异常”都被执行一段代码；

## 常见的runtime exception
1、NullPointerException  空指针引用异常
2、ClassCastException - 类型强制转换异常。
3、IllegalArgumentException - 传递非法参数异常。
4、IndexOutOfBoundsException - 下标越界异常
5、UnsupportedOperationException - 不支持的操作异常
6、ArithmeticException - 算术运算异常
7、ArrayStoreException - 向数组中存放与声明类型不兼容对象异常
8、NegativeArraySizeException - 创建一个大小为负数的数组错误异常
9、NumberFormatException - 数字格式异常
10、SecurityException - 安全异常

## throw和throws有什么区别
throw关键字用来在程序中明确的抛出异常，相反，throws语句用来表明方法不能处理的异常。每一个方法都必须要指定哪些异常不能处理，所以方法的调用者才能够确保处理可能发生的异常，多个异常是用逗号分隔的。

## Switch能否用string做参数？
Java 1.7之前不可以，java 1.7后String可以作为参数。
整型（byte，short int，int，long int），枚举类型，boolean，字符型(char),字符串都可以，唯独浮点型不可以

## equals与==的区别
1、 == 是一个运算符。 
2、Equals则是string对象的方法，可以.（点）出来。 
我们比较无非就是这两种 1、基本数据类型比较 2、引用对象比较 
1、基本数据类型比较 
　==比较两个值是否相等。相等为true 否则为false；
 equals不能直接用于基本类型的比较。需要将基本类型转换为包装器进行比较。 
2、引用对象比较 
==和equals都是比较栈内存中的地址是否相等 。相等为true 否则为false； 　 
需注意几点： 
1、string是一个特殊的引用类型。对于两个字符串的比较，不管是 == 和 equals 这两者比较的都是字符串是否相同； 
2、当你创建两个string对象时，内存中的地址是不相同的，你可以赋相同的值。 
　　所以字符串的内容相同。引用地址不一定相同，（相同内容的对象地址不一定相同），但反过来却是肯定的； 
3、基本数据类型比较(string 除外) == 和 Equals 两者都是比较值；

## Object有哪些公用方法
protected Object clone()创建并返回此对象的一个副本。 
public boolean equals(Object obj)指示其他某个对象是否与此对象“相等”。 
protected void finalize()当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。 
public final native Class< ? > getClass() 返回此 Object 的运行时类。
public int hashCode()返回该对象的哈希码值。 
public String toString()返回该对象的字符串表示。 
public void notify()唤醒在此对象监视器上等待的单个线程。 
public void notifyAll()唤醒在此对象监视器上等待的所有线程。 
public void wait()在其他线程调用此对象的 notify() 方法或 notifyAll() 方法前，导致当前线程等待。 
public void wait(long timeout)在其他线程调用此对象的 notify() 方法或 notifyAll() 方法，或者超过指定的时间量前，导致当前线程等待。 
public void wait(long timeout, int nanos)在其他线程调用此对象的 notify() 方法或

注意：
1.如果一个类实现了Cloneable,Object的clone方法就返回该对象的逐域拷贝，否则就会抛出CloneNotSupportException异常。java.lang.Cloneable 是一个标示性接口，不包含任何方法（详见《effective java》 p46）。
2. 覆盖equals（）时总要覆盖hashCode（）（详见《effective java》p39）
在每个覆盖equals方法的类中也必须覆盖hashCode方法，如果不这样做就会导致Object.hashCode的通用约定---**相等的对象必须具有相等的散列码的约定**，从而导致该类无法集合所有基于散列集合一起工作，这样的集合有HashMap,HashSet和HashTable。HashMap等使用Key对象的hashCode()和equals()方法去决定key-value对的索引。如果将equals相等的对象认为是同一个对象的话，那么put方法将对象放在一个散列桶，而get方法可能从另一个散列桶获取该对象，因为这两个方法传入的对象虽然equals相同，但hashCode可能不同，而hashMap根据hashCode去定位散列桶位置导致出现在不同的散列桶中。
 Hashcode的作用。
1. hashCode的存在主要是用于查找的快捷性，如Hashtable，HashMap等，hashCode是用来在散列存储结构中确定对象的存储地址的；
2. 比较对象是否相同。
一下是关于hashCode的约定：
1、如果两个对象相同，就是适用于equals(java.lang.Object) 方法，那么这两个对象的hashCode一定要相同；
2、如果对象的equals方法被重写，那么对象的hashCode也尽量重写，并且产生hashCode使用的对象，一定要和equals方法中使用的一致，否则就会违反上面提到的第2点；
3、两个对象的hashCode相同，并不一定表示两个对象就相同，也就是不一定适用于equals(java.lang.Object) 方法，只能够说明这两个对象在散列存储结构中，如Hashtable，他们“存放在同一个篮子里”。

## String、StringBuffer与StringBuilder的区别
1. String是字符串常量，是不可变类。如果要操作少量的数据用
2. StringBuffer是字符串变量，是线程安全的。多线程操作字符串缓冲区 下操作大量数据
3. StringBuilder是字符串变量，非线程安全。单线程操作字符串缓冲区 下操作大量数据
速度：StringBuilder >  StringBuffer  >  String
String 对象的字符串拼接其实是被 JVM 解释成了 StringBuffer 对象的拼接，所以这些时候 String 对象的速度并不会比 StringBuffer 对象慢，而特别是以下的字符串对象生成中， String 效率是远要比 StringBuffer 快的：
String S1 = “This is only a” + “ simple” + “ test”;
StringBuffer Sb = new StringBuilder(“This is only a”).append(“simple”).append(“ test”);
参见：http://www.findspace.name/easycoding/1090

## try catch finally，try里有return，finally还执行么？
1)不管有木有出现异常，finally块中代码都会执行 
2)当try和catch中有return时，finally仍然会执行 
3)finally是在return后面的表达式运算后执行的（此时并没有返回运算后的值，而是先把要返回的值保存起来，**不管finally中的代码怎么样，返回的值都不会改变**，任然是之前保存的值），所以函数返回值是在finally执行前确定的 
4)finally中最好不要包含return，否则程序会提前退出，返回值不是try或catch中保存的返回值

## UnsupportedOperationException是什么
UnsupportedOperationException是用于表明操作不支持的异常。在JDK类中已被大量运用，在集合框架java.util.Collections.UnmodifiableCollection将会在所有add和remove操作中抛出这个异常。


##  Excption与Error包结构


## java值传递问题
1.对象就是传引用
2.原始类型就是传值
3.String，Integer, Double等immutable类型因为没有提供自身修改的函数，每次操作都是新生成一个对象，所以要特殊对待。可以认为是传值。
Integer 和 String 一样。保存value的类变量是Final属性，无法被修改，只能被重新赋值／生成新的对象。 当Integer 做为方法参数传递进方法内时，对其的赋值都会导致 原Integer 的引用被 指向了方法内的栈地址，失去了对原类变量地址的指向。对赋值后的Integer对象做得任何操作，都不会影响原来对象。


## Integer
http://www.cnblogs.com/dolphin0520/p/3780005.html

## 修饰符顺序
public protected private abstract static final transient volatile synchronized native strictfp


个人公众号(欢迎关注)：<br>
![](/assets/weix_gongzhonghao.jpg)



