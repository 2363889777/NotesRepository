# lambda表达式

## 一、lambda基础

### 1）了解lambda语法

#### Ⅰ、为什么会有lambda语法

大量存在这样的场景，

​		要实现某个功能需要实现某个**只有一个方法的接口**中的那个方法，且这个方法体可能只有几行代码（例如，比较Comparator接口....）,在jdk8之前只能使用内部的方式来进行代码的阅读优化，但仍然为阅读提供了障碍;

​		所以迫切需求某种方法来解决该类问题（功能性代码少，但是使用该功能性代码的过程复杂且重复多余），lambda就是解决该类问题的一种方式！

​	lambda的使用环境：https://www.jianshu.com/p/0ffa278deb44

<details>
  <summary>1.列表迭代</summary>  
   对一个列表的每一个元素进行操作，不使用 Lambda 表达式时如下：
	<pre><code>
    List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
        for (int element : numbers) {
            System.out.prinln(element);
        }
	</code></pre>
	使用 Lambda 表达式：
	<pre><code>
    List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
    numbers.forEach(x -> System.out.println(x));
    </code></pre>
	如果只需要调用单个函数对列表元素进行处理，那么可以使用更加简洁的 方法引用(方法参考：下面会具体深入) 代替 Lambda 表达式：
    <pre><code>
    List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
    numbers.forEach(System.out::println);
    </code></pre>
</details>

<details>
  <summary>2.事件监听</summary>  
    不使用 Lambda 表达式：
    <pre><code>
        button.addActionListener(new ActionListener(){
        @Override
        public void actionPerformed(ActionEvent e) {
            //handle the event
        }
    });
    </code></pre>
    使用 Lambda 表达式，需要编写多条语句时用花括号包围起来：
    <pre><code>
        button.addActionListener(e -> {
            //handle the event
        });
    </code></pre>
</details>

<details>
  <summary>3.Predicate 接口</summary>  
    java.util.function 包中的 Predicate 接口可以很方便地用于过滤。如果你需要对多个对象进行过滤并执行相同的处理逻辑，那么可以将这些相同的操作封装到 filter 方法中，由调用者提供过滤条件，以便重复使用。不使用 Predicate 接口，对于每一个对象，都需要编写过滤条件和处理逻辑：
    <pre><code>
    List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
	List<String> words = Arrays.asList("a", "ab", "abc");numbers.forEach(x -> {
    if (x % 2 == 0) {
        //process logic
        }
    })
    words.forEach(x -> {
        if (x.length() > 1) {
            //process logic
        }
    })
	</code></pre>
使用 Predicate 接口，将相同的处理逻辑封装到 filter 方法中，重复调用：
<pre><code>
	public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
        List<String> words = Arrays.asList("a", "ab", "abc");
        filter(numbers, x -> (int)x % 2 == 0);
        filter(words, x -> ((String)x).length() > 1);
	}
	public static void filter(List list, Predicate condition) {
        list.forEach(x -> {
            if (condition.test(x)) {
                //process logic
            }
        })
    }
</code></pre>
filter 方法也可写成：
<pre><code>
    public static void filter(List list, Predicate condition) {
        list.stream().filter(x -> condition.test(x)).forEach(x -> {
            //process logic
        })
    }
</code></pre>
</details>

<details>
  <summary>4.Map 映射</summary>  
    Reduce 聚合、代替 Runnable
    <br>
	详细情况：https://www.jianshu.com/p/0ffa278deb44
</details>

<details>
  <summary>5.Reduce 聚合</summary>  
    代替 Runnable
    <br>
	详细情况：https://www.jianshu.com/p/0ffa278deb44
</details>

<details>
  <summary>6.代替 Runnable</summary>  
    <br>
	详细情况：https://www.jianshu.com/p/0ffa278deb44
</details>	

#### Ⅱ、lambda的注意点

1. lambda一般用来实现只有一个方法的接口的方法

2. lambda中禁止修改所引用到的局部变量的值

   <details>
     <summary>Lambda 遇上 this和 final之 final</summary>  
   	<pre><code>
   		package com.zuixue.lambdacase;
           import java.util.Arrays;
           import java.util.Collection;
           /**
            * @类: FinalCase
            * @描述: Lambda 遇上 this和 final之 final
            * @date: 2020/7/17
            * @author: Admin
            * @ver 1.0.0
            * @since JDK 1.8
            */
           public class FinalCase {
               /**
                * 被lambda所用到的变量，必须是final性质的（直接引用不会被修改），
                * 所以不论是lambda内部还是外部，当lambda使用了这个变量，这个变量就不能改变，但是可
                * 以修改内容
                * @param args
                */
               public static void main(String[] args) {
                   String[] names = new String[]{"张三","李四","王五","赵六"};
                   Runnable runnable = ()-> {
                       Arrays.asList(names).forEach((name)-> System.out.println(name));
           //            names =  new String[]{"张三","李四","王五","赵六"};(编译报错)
                   };
                   runnable.run();
                   System.out.println("不修改直接引用，修改内容------------->");
           //        names =  new String[]{"张三","李四","王五","赵六"};(编译报错：Variable         //        used in lambda expression should be final or effectively final)
                   names[0] = "侯七";
                   runnable.run();
               }
           }
   	</code></pre>
       结果：<br>
       	张三
           李四
           王五
           赵六<br>
           不修改直接引用，修改内容-------------><br>
           侯七
           李四
           王五
           赵六
   </details>	

3. lambda中this是根据上下文环境确定的

   <details>
     <summary> Lambda 遇上 this和 final 之 this</summary>  
       <pre><code>
       package com.zuixue.lambdacase;
       /**
        * @类: ThisAndFinalCase
        * @描述: Lambda 遇上 this和 final 之 this
        * @date: 2020/7/17
        * @author: Admin
        * @ver 1.0.0
        * @since JDK 1.8
        */
       public class ThisCase {
           /**
            * lambda不是匿名类的语法蜜糖
            * 论证：
            *      如果lambda是匿名类的语法蜜糖，那么打印的this会相同
            *      反之，如果不是则不相同
            */
           /**
            * 内部类 打印this
            */
           Runnable runnable1 = new Runnable() {
               @Override
               public void run() {
                   System.out.println(this);
               }
           };
           /**
            * 内部类 本身没有重写toString
            */
           Runnable runnable2 = new Runnable() {
               @Override
               public void run() {
                   System.out.println(toString());
               }
           };
           /**
            * 内部类 本身重写toString
            */
           Runnable runnable3 = new Runnable() {
               @Override
               public void run() {
                   System.out.println(toString());
               }
               @Override
               public String toString() {
                   return "匿名类3";
               }
           };
           Runnable runnable1_1 = ()->System.out.println(this);
           Runnable runnable2_1 = ()-> System.out.println(toString());
           @Override
           public String toString() {
               return "Hello World";
           }
           public static void main(String[] args) {
               ThisCase thisCase = new ThisCase();
               System.out.println("内部类的三种情况--------------------->");
               System.out.println("1.打印内部类的this");
               thisCase.runnable1.run();
               System.out.println("内部类的外部类："+ thisCase);
               System.out.println("2.内部类 本身没有重写toString");
               thisCase.runnable2.run();
               System.out.println("3.内部类 本身重写toString");
               thisCase.runnable3.run();
               System.out.println("使用lambda----------------------->");
               System.out.println("1.打印简化匿名内部类的this");
               thisCase.runnable1_1.run();
               System.out.println("2.匿名内部类 本身没有重写toString");
               thisCase.runnable2_1.run();
               System.out.println("结论：lambda不是匿名类的语法蜜糖，\n" +
                       "在lambda中使用this是看lambda所处的上下文环境,不是指lambda所简化的匿名				类");
           }
       }
       </code></pre>
       打印结果：<br>
       	内部类的三种情况---------------------><br>
           1.打印内部类的this<br>
           com.zuixue.lambdacase.ThisCase$1@3d075dc0<br>
           内部类的外部类：Hello World<br>
           2.内部类 本身没有重写toString<br>
           com.zuixue.lambdacase.ThisCase$2@214c265e<br>
           3.内部类 本身重写toString<br>
           匿名类3<br>
           使用lambda-----------------------><br>
           1.打印简化匿名内部类的this<br>
           Hello World<br>
           2.匿名内部类 本身没有重写toString<br>
           Hello World<br>
           结论：lambda不是匿名类的语法蜜糖，<br>
           在lambda中使用this是看lambda所处的上下文环境,不是指lambda所简化的匿名类<br>
   </details>

#### Ⅲ、lambda语法的具体格式

```java
/**
 * 参考：https://www.runoob.com/java/java8-lambda-expressions.html
 *
 * 注意点：
 *      1.一般用于实现只有单方法的接口的方法
 *      2.参数类型如果程序可以推断出类型，则可以不写lambda表达式的参数类型
 *
 * // 1. 不需要参数,返回值为 5
 * () -> 5
 * 
 * // 2. 接收一个参数(数字类型),返回其2倍的值
 * x -> 2 * x
 * 
 * // 3. 接受2个参数(数字),并返回他们的差值
 * (x, y) -> x – y
 * 
 * // 4. 接收2个int型整数,返回他们的和
 * (int x, int y) -> x + y
 * 
 * // 5. 接受一个 string 对象,并在控制台打印,不返回任何值(看起来像是返回void)
 * (String s) -> System.out.print(s)
 */
```

### 2）方法参考

#### Ⅰ、为什么会有方法参考

存在以下这些情况，

1. 某些方法的操作流程与你定义的lambda表达式相同（使用lambda会使代码冗余，且在尽可能的情况下应当使用已有的api）

   方法分为：静态方法和对象的实例方法

   

#### Ⅱ、方法参考的注意点

可以将方法参考理解为lambda的一种补充，在lambda的基础上再一次进行优化

#### Ⅲ、方法参考的具体格式

~~~java
 public static void main(String[] args) {
        List<String> s = Arrays.asList("cc","aaa","bbb");
        //方法参考案例： System.out::println  （对象::方法）
        s.forEach(System.out::println);
    }
~~~

###  3）了解接口默认方法



### 4）疑惑补充

#### 1 ForEach

经常看见这样的代码

~~~java
 public static void main(String[] args) {
        List<String> s = Arrays.asList("cc","aaa","bbb");
        //方法参考案例： System.out::println  （对象::方法）
        s.forEach(System.out::println);
 }
~~~

这里就是匿名实现了Consumer接口的accept方法

##### 源码分析

<details>
  <summary>Iterable<T> </summary>  
    <pre><code>
     /*
     * Copyright (c) 2003, 2013, Oracle and/or its affiliates. All rights reserved.
     * ORACLE PROPRIETARY/CONFIDENTIAL. Use is subject to license terms.
     */
    package java.lang;
    import java.util.Iterator;
    import java.util.Objects;
    import java.util.Spliterator;
    import java.util.Spliterators;
    import java.util.function.Consumer;
    /**
     * Implementing this interface allows an object to be the target of
     * the "for-each loop" statement. See
     * <strong>
     * <a href="{@docRoot}/../technotes/guides/language/foreach.html">For-each Loop</a>
     * </strong>
     *
     * @param <T> the type of elements returned by the iterator
     *
     * @since 1.5
     * @jls 14.14.2 The enhanced for statement
     */
    public interface Iterable<T> {
        /**
         * Returns an iterator over elements of type {@code T}.
         *
         * @return an Iterator.
         */
        Iterator<T> iterator();
        /**
         * Performs the given action for each element of the {@code Iterable}
         * until all elements have been processed or the action throws an
         * exception.  Unless otherwise specified by the implementing class,
         * actions are performed in the order of iteration (if an iteration order
         * is specified).  Exceptions thrown by the action are relayed to the
         * caller.
         *
         * @implSpec
         * <p>The default implementation behaves as if:
         * <pre>{@code
         *     for (T t : this)
         *         action.accept(t);
         * }</pre>
         *
         * @param action The action to be performed for each element
         * @throws NullPointerException if the specified action is null
         * @since 1.8
         */
        default void forEach(Consumer<? super T> action) {
            Objects.requireNonNull(action);
            for (T t : this) {
                action.accept(t);
            }
        }
        /**
         * Creates a {@link Spliterator} over the elements described by this
         * {@code Iterable}.
         *
         * @implSpec
         * The default implementation creates an
         * <em><a href="Spliterator.html#binding">early-binding</a></em>
         * spliterator from the iterable's {@code Iterator}.  The spliterator
         * inherits the <em>fail-fast</em> properties of the iterable's iterator.
         *
         * @implNote
         * The default implementation should usually be overridden.  The
         * spliterator returned by the default implementation has poor splitting
         * capabilities, is unsized, and does not report any spliterator
         * characteristics. Implementing classes can nearly always provide a
         * better implementation.
         *
         * @return a {@code Spliterator} over the elements described by this
         * {@code Iterable}.
         * @since 1.8
         */
        default Spliterator<T> spliterator() {
            return Spliterators.spliteratorUnknownSize(iterator(), 0);
        }
    }
    </code></pre>
</details>	

~~~java
  default void forEach(Consumer<? super T> action) {
            Objects.requireNonNull(action);
            for (T t : this) {
                action.accept(t);
            }
        }
~~~

<details>
  <summary>Consumer<T></summary>  
  <pre><code>
      /*
     * Copyright (c) 2010, 2013, Oracle and/or its affiliates. All rights reserved.
     * ORACLE PROPRIETARY/CONFIDENTIAL. Use is subject to license terms.
     */
    package java.util.function;
    import java.util.Objects;
    /**
     * Represents an operation that accepts a single input argument and returns no
     * result. Unlike most other functional interfaces, {@code Consumer} is expected
     * to operate via side-effects.
     *
     * <p>This is a <a href="package-summary.html">functional interface</a>
     * whose functional method is {@link #accept(Object)}.
     *
     * @param <T> the type of the input to the operation
     *
     * @since 1.8
     */
    @FunctionalInterface
    public interface Consumer<T> {
        /**
         * Performs this operation on the given argument.
         *
         * @param t the input argument
         */
        void accept(T t);
        /**
         * Returns a composed {@code Consumer} that performs, in sequence, this
         * operation followed by the {@code after} operation. If performing either
         * operation throws an exception, it is relayed to the caller of the
         * composed operation.  If performing this operation throws an exception,
         * the {@code after} operation will not be performed.
         *
         * @param after the operation to perform after this operation
         * @return a composed {@code Consumer} that performs in sequence this
         * operation followed by the {@code after} operation
         * @throws NullPointerException if {@code after} is null
         */
        default Consumer<T> andThen(Consumer<? super T> after) {
            Objects.requireNonNull(after);
            return (T t) -> { accept(t); after.accept(t); };
        }
    }
  </code></pre>
</details>	

```
void accept(T t);
```



## 二、lambda进阶

