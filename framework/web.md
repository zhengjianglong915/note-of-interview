# web 部分
## 1. servlet生命周期
servlet的声明周期周期是由servlet的容器来控制,它可以分为3个阶段:初始化,运行和销毁. 

### 1) 初始化阶段 
初始化阶段主要完成以下任务: 

- servlet容器加载servlet类,把servlet类的.class文件中的数据读到内存中. 
- servlet容器创建一个ServletConfig对象,ServletConfig对象包含了servlet的初始化配置信息. 
- servlet容器创建一个servlet对象 
- servlet容器调用servlet的init方法进行初始化. 

### 2) 运行阶段 
当servlet容器收到一个请求时,servlet容器会针对这个请求创建servletRequest和servletResponse对象,然后调用service方法.并把这两个参数传递给service方法.service方法通过servletRequest对象获得请求的信息,并处理该请求. 再通过servletResponse对象生成这个请求的相应结果.然后销毁servletRequest和servletResponse对象. 不管这个请求时post还是get提交的,最终这个请求都会由service方法来处理. 

### 3) 销毁阶段 
当web应用被终止时,servlet容器会调用servlet对象的destroy方法,然后销毁servlet对象. 同时也会销毁servlet对象相关联的servletConfig对象. 我们可以通过destroy方法释放servlet占用的资源.

