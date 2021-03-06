# 集合+MAP+IO

## 一、集合

![集合结构图](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200602193057.png)



## 二、MAP

![Map结构图](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200602193310.png)



### （一）HashMap

##### 1.HashMap的数据结构

​	jdk版本不同结构不同：

- ​	1.7：数组+链表

- ​	1.8：数组+链表+红黑树

##### 2.HashMap实现原理

##### 3.HashMap源码解析

## 三、IO

![image-20200602193929704](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200602193929.png)

![流结构图](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200602194109.png)

#### 1.有了字节流为什么还有字符流？

![有了字节流为什么还有字符流](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200602195352.png)

#### 2.**BIO,NIO,AIO** **有什么区别**?

Java 中的 BIO、NIO和 AIO 理解为是 Java 语言对操作系统的各种 IO 模型的封装。程序员在使用这些 API 的时候，不需要关心操作系统层面的知识，也不需要根据不同操作系统编写不同的代码。只需要使用Java的API就可以了。

在讲 BIO,NIO,AIO 之前先来回顾一下这样几个概念：同步与异步，阻塞与非阻塞。

**同步与异步**

- **同步：** 同步就是发起一个调用后，被调用者未处理完请求之前，调用不返回。
- **异步：** 异步就是发起一个调用后，立刻得到被调用者的回应表示已接收到请求，但是被调用者并没有返回结果，此时我们可以处理其他的请求，被调用者通常依靠事件，回调等机制来通知调用者其返回结果。

同步和异步的区别最大在于异步的话调用者不需要等待处理结果，被调用者会通过回调等机制来通知调用者其返回结果。

**阻塞和非阻塞**

- **阻塞：** 阻塞就是发起一个请求，调用者一直等待请求结果返回，也就是当前线程会被挂起，无法从事其他任务，只有当条件就绪才能继续。
- **非阻塞：** 非阻塞就是发起一个请求，调用者不用一直等着结果返回，可以先去干其他事情。

**那么同步阻塞、同步非阻塞和异步非阻塞又代表什么意思呢？**

举个生活中简单的例子，你妈妈让你烧水，小时候你比较笨啊，在哪里傻等着水开（**同步阻塞**）。等你稍微再长大一点，你知道每次烧水的空隙可以去干点其他事，然后只需要时不时来看看水开了没有（**同步非阻塞**）。后来，你们家用上了水开了会发出声音的壶，这样你就只需要听到响声后就知道水开了，在这期间你可以随便干自己的事情，你需要去倒水了（**异步非阻塞**）。

BIO (Blocking I/O)、NIO (New I/O)、AIO (Asynchronous I/O)

![image-20200602195137355](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200602195137.png)

















































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































