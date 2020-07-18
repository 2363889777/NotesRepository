# java-JVM

参考：https://www.cnblogs.com/yuechuan/p/8984262.html

​				https://blog.csdn.net/Luomingkui1109/article/details/72820232

## 一、编译顺序

![编译顺序](https://images2017.cnblogs.com/blog/352511/201708/352511-20170810232430449-973255879.png)



![编译顺序](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200601114822.png)



## 二、方法执行顺序

![方法执行顺序](https://images2017.cnblogs.com/blog/352511/201708/352511-20170810232431824-998419876.png)

## 三、jvm结构图

![jvm结构图](https://images2017.cnblogs.com/blog/352511/201708/352511-20170810232433792-373676900.png)

![img](https://img-blog.csdnimg.cn/20200506000229203.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x1b21pbmdrdWkxMTA5,size_16,color_FFFFFF,t_70)

![jvm运行时数据区](https://img-blog.csdnimg.cn/20200506000229233.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0x1b21pbmdrdWkxMTA5,size_16,color_FFFFFF,t_70)



内存空间：

 JVM内存空间包含：方法区、java堆、java栈、本地方法栈。

**方法区**是各个线程共享的区域，存放类信息、常量、静态变量。

**java堆**也是线程共享的区域，我们的类的实例就放在这个区域，可以想象你的一个系统会产生很多实例，因此java堆的空间也是最大的。如果java堆空间不足了，程序会抛出OutOfMemoryError异常。

**java栈**是每个线程私有的区域，它的生命周期与线程相同，一个线程对应一个java栈，每执行一个方法就会往栈中压入一个元素，这个元素叫“栈帧”，而栈帧中包括了方法中的局部变量、用于存放中间状态值的操作栈，这里面有很多细节，我们以后再讲。如果java栈空间不足了，程序会抛出StackOverflowError异常，想一想什么情况下会容易产生这个错误，对，递归，递归如果深度很深，就会执行大量的方法，方法越多java栈的占用空间越大。

每个帧代表一个方法，Java方法有两种返回方式，return和抛出异常，两种方式都会导致该方法对应的帧出栈和释放内存。

 ③ 栈运行原理

   栈中的数据都是以栈帧（Stack Frame）的格式存在，栈帧是一个内存区块，是一个数据集，是一个有关方法和运行期数据的数据集，当一个方法A被调用时就产生了一个栈帧F1，并被压入到栈中，A方法又调用了B方法，于是产生栈帧F2也被压入栈，B方法又调用了C方法，于是产生栈帧F3也被压入栈…… 依次执行完毕后，先弹出后进......F3栈帧，再弹出F2栈帧，再弹出F1栈帧。

   遵循“先进后出”/“后进先出”原则。

  帧的组成：局部变量区（包括方法参数和局部变量，对于instance方法，还要首先保存this类型，其中方法参数按照声明顺序严格放置，局部变量可以任意放置），操作数栈，帧数据区（用来帮助支持常量池的解析，正常方法返回和异常处理）。

本地方法栈角色和java栈类似，只不过它是用来表示执行本地方法的，本地方法栈存放的方法调用本地方法接口，最终调用本地方法库，实现与操作系统、硬件交互的目的。

PC寄存器，说到这里我们的类已经加载了，实例对象、方法、静态变量都去了自己改去的地方，那么问题来了，程序该怎么执行，哪个方法先执行，哪个方法后执行，这些指令执行的顺序就是PC寄存器在管，它的作用就是控制程序指令的执行顺序。

执行引擎当然就是根据PC寄存器调配的指令顺序，依次执行程序指令。

静态变量+常量+类信息+运行时常量池存在方法区中，实例变量存在堆内存中。

 基本类型的变量和对象的引用变量都是在函数的栈内存中分配。

#### 拓展

##### （一）关于Object o = new Object()

![image-20200605092749938](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200605092757.png)

###### 1.请解释一下对象的创建过程？（半初始化）

![初始化过程](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200605093331.png)

###### 2.加问DCL与volatile问题？(指令重排)

DCL:double check lock

![DCL:double check lock](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200605095115.png)

volatile:

1. 保持线程可见性
2. 禁止指令重排序

![使用volatile原因](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200605102139.png)

###### 3.对象与数组的存储不同

![对象在内存中存储布局](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200605102743.png)

###### 4.对象头具体包括什么？（markword klasspointer） synchronized锁信息

###### 5.对象怎么定位

​	![image-20200605110918442](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200605110918.png)

**优缺点分析：**

- 句柄方式
  - 优点：效率高
  - 缺点：GC垃圾回收机制时，需要修改对象的引用
- 直接引用
  - 优点：GC实现方便，不用改对象，只用改实例数据指针（GC就是把一个对象从一个区域复制到另一个区域并且将原区域删除）

###### 8.class对象是在堆还是在方法区

![jvm结构图](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200605121745.png)

**总结：**

1. 1.7是在方法区
2. 1.8中hotspot是使用OOP-KLASS模型表示JAVA对象，在方法区创建一个InstanceClassOop（C++对象），通过java的反射在堆中创建Class对象（java对象），实现java对象和java对象的调用
   - 为什么不用C++对象直接表示java对象
     - C++对象表示java对象需要虚方法表，如果直接使用C++对象表示java对象，太过庞大，所以分了两个

##### （二）查看java对象的内存结构布局

![image-20200605103245475](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200605103245.png)

![image-20200605115949852](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200605115950.png)

**jol的使用**

1.基础使用

![image-20200605103738544](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200605103738.png)

![image-20200605103554499](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200605103554.png)

2.加锁

![image-20200605104152565](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200605104152.png)

![image-20200605104317150](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20200605104317150.png)