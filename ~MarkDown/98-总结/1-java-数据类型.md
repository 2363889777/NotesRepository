# java-数据类型

## 一、自动装箱、自动拆箱

自动装箱是 Java 编译器在基本数据类型和对应的对象包装类型之间做的

一个转化。比 如：

把 int 转化成 Integer，double 转化成 Double，等等。反之就是自动拆箱。 

原始类型: boolean，char，byte，short，int，long，float，double 

封装类型：Boolean，Character，Byte，Short，Integer，Long，Float，Double

## 二、String

string、 StringBuffer、StringBuilder
都是final类都不允许被继承
string长度是不可变的, String Buffer、 StringBuilder长度是可变的;
StringBuffer是线程安全的 String Builder不是线程安全的,但它们两个中的所有方法是相同的, StringBuffer在
String Builder的方法之上添加了 synchronized修饰,保证线程安全 
String Builder比 StringBuffer有更好的性能 
如果一个 String类型的字符串,在编译时就可以确定是一个字符串常量,则编译完成之后,字符串会自动拼接成一个常量。此时 String的速度比 String Buffer 和 StringBuilder 的性能好得的多

![image-20200601210342289](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200601210343.png)

![image-20200601210522331](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200601210522.png)

String 中的 hashcode 以及 toString

![image-20200601210902326](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200601210902.png)

![image-20200601210927913](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200601210928.png)

String，是否可以继承，“+”怎样实现，与 StringBuffer 区别

![image-20200601211014599](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200601211014.png)

