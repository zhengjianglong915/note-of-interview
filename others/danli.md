> 《设计模式》提出近二十年里，随着面向对象语言的发展，单例模式也随之演化，如今其实现形式变得多种多样。常见的单例模式有懒汉、饿汉、双重校验锁、枚举和静态内部类五种形式。
双重校验锁DCL(double checked locking)

## 1.双重校验
双重校验锁式（也有人把双重校验锁式和懒汉式归为一类）分别在代码锁前后进行判空校验，避免了多个有机会进入临界区的线程都创建对象，同时也避免了代码段1-4后来线程在先来线程创建对象后但仍未退出临界区的情况下等待。双重校验锁代码如下：

    public class Singleton{
       private volatile static Singleton singleton = null;    //注意此处加上了volatile关键字
         
        private Singleton(){//这边不能省略
        }
         
        public static Singleton getInstance(){
            if(singleton == null){
                synchronized(Singleton.class){
                    if(singleton == null){
                        singleton = new Singleton();
                        //return singleton;    //有人提议在此处进行一次返回
                    }
                    //return singleton;    //也有人提议在此处进行一次返回
                }
            }
            return singleton;
        }
    }


经多次试验说明，双重校验锁式是线程安全的。然而，在JDK1.5以前，DCL是不稳定的，有时也可能创建多个实例，在1.5以后开始提供volatile关键字修饰变量来达到稳定效果。

## 2.饿汉式单例  
单例模式的饿汉式，在定义自身类型的成员变量时就将其实例化，使得在Singleton单例类被系统（姑且这么说）加载时就已经被实例化出一个单例对象，从而一劳永逸地避免了线程安全的问题。代码如下：

    public class Singleton {
       private final static Singleton INSTANCE = new Singleton();
       private Singleton(){ }
       public static Singleton getInstance() {
          return INSTANCE;
       }
    }
    
ClassLoader加载Singleton类时，饿汉式单例就被创建,虽然饿汉式单例是线程安全的，但也有其不足之处。饿汉式单例在类被加载时就创建单例对象并且长驻内存，不管你需不需要它；如果单例类占用的资源比较多，就会降低资源利用率以及程序的运行效率。有一种更高级的单例模式则很好地解决了这个问题——静态内部类。

##3. 静态内部类
静态内部类式和饿汉式一样，同样利用了ClassLoader的机制保证了线程安全；不同的是，饿汉式在Singleton类被加载时（从代码段3-2的Class.forName可见）就创建了一个实例对象，而静态内部类即使Singleton类被加载也不会创建单例对象，除非调用里面的getInstance()方法。因为当Singleton类被加载时，其静态内部类SingletonHolder没有被主动使用。只有当调用getInstance方法时，才会装载SingletonHolder类，从而实例化单例对象。

    public class Singleton {
        public static Singleton getInstance(){
            return SingletonHolder.instance;
        }
        private Singleton(){}
        private static class SingletonHolder{
            private static Singleton instance = new Singleton();
        }
    }
    
通过静态内部类的方法就实现了lazy loading，很好地将懒汉式和饿汉式结合起来，既实现延迟加载，保证系统性能，也能保证线程安全。

##4.枚举单例
上面说到的静态内部类方式不失为一个高级的单例模式实现。但如果开发要求更严格一些，比如你的Singleton类实现了序列化，又或者想避免通过反射来破解单例模式的话，单例模式还可以有另一种形式。那就是枚举单例。枚举类型在JDK1.5被引进。这种方式也是《Effective Java》作者Josh Bloch 提倡的方式，它不仅能避免多线程的问题，而且还能防止反序列化重新创建新的对象、防止被反射攻击。代码如下：

    public enum EnumSingleton {
        INSTANCE{
            @Override
            protected void work() {
                System.out.println("你好，是我!");
            }
             
        };
         
        protected abstract void work();    //单例需要进行操作（也可以不写成抽象方法）
    }


在外部，可以通过EnumSingleton.INSTANCE.work()来调用work方法。默认的枚举实例的创建是线程安全的，但是实例内的各种方法则需要程序员来保证线程安全。总的来说，使用枚举单例模式，有三个好处：1.实例的创建线程安全，确保单例。2.防止被反射创建多个实例。3.没有序列化的问题

##5.懒汉式
在需要的时候才创建单例对象，而不是随着软件系统的运行或者当类被加载器加载的时候就创建。当单例类的创建或者单例对象的存在会消耗比较多的资源，常常采用lazy loading策略。这样做的一个明显好处是提高了软件系统的效率，节约内存资源。

    public class Singleton {
        private static Singleton singleton = null;    
         
        private Singleton(){
            System.out.println("构造函数被调用");
        }
         
        public static Singleton getInstance(){
                synchronized(Singleton.class){
                    if(singleton == null){
                    singleton = new Singleton();
                }
            }
            return singleton;
        }
    }
然而这样做类似于在方法签名上加上synchronized关键字，会影响程序效率。因为当有多个线程几乎同时访问getInstance方法时，多个线程必须有次序地进入方法内，这样导致了若干个线程需要耗费等待进入临界区（被锁住的代码块）的时间。基于此，有人提出了双重校验锁式。

## 参考资料
http://blog.chenzuhuang.com/archive/13.html

个人公众号(欢迎关注)：<br>
![](/assets/weix_gongzhonghao.jpg)


