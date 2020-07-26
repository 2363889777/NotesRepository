# 枚举的序列化

## 一、基础常识

### 1. 什么是序列化？反序列化？

> Java 序列化就是指将对象转换为字节序列的过程，而反序列化则是只将字节序列转换成目标对象的过程。

> seriallization 序列化 ： 将对象转化为便于传输的格式， 常见的序列化格式：二进制格式，字节数组，json字符串，xml字符串。
>  deseriallization 反序列化：将序列化的数据恢复为对象的过程。

举个例子：
 比如：现在我们都会在淘宝上买桌子，桌子这种很不规则不东西，该怎么从一个城市运输到另一个城市，这时候一般都会把它拆掉成板子，再装到箱子里面，就可以快递寄出去了，这个过程就类似我们的**序列化**的过程（把数据转化为可以存储或者传输的形式）。当买家收到货后，就需要自己把这些板子组装成桌子的样子，这个过程就像**反序列** 的过程（转化成当初的数据对象）。

### 2、为什么要序列化？

我们都知道，在进行浏览器访问的时候，我们看到的文本、图片、音频、视频等都是通过二进制序列进行传输的，那么如果我们需要将Java对象进行传输的时候，是不是也应该先将对象进行序列化？答案是肯定的，我们需要先将Java对象进行序列化，然后通过网络，IO进行传输，当到达目的地之后，再进行反序列化获取到我们想要的对象，最后完成通信。

## 二、枚举的序列化问题的出现与解决

### 1.出现的环境

1. 我们web数据传输普遍使用Jackson来实现序列化，在Spring中使用`@RestController`注解完成返回结果对象的自动json序列化。

2. 我们现在习惯于使用枚举来代替常量，我们肯定不仅仅满足于这样使用枚举

   ```java
   public enum Eunm1 {
       /**
        * 定义的常量
        */
       A,B;
   
       Eunm1() {
       }
   }
   ```

   我们一般会希望在枚举中添加属性来实现更好的使用体验

   ```java
   @AllArgsConstructor
   @Getter
   public enum Eum2 {
       /**
        * 枚举常量
        */
       A("情况1",200),B("情况2",500);
   
       private String name;
   
       private Integer num;
   }
   ```

### 2.产生的问题：

将其转换为json时，只包含给枚举量的名称，不包含属性，这往往不是我们想要的

```
@RestController
public class EnumTest {

    @GetMapping("/eunm")
    public Eunm2 getEunm2() {
        return Eunm2.A;
    }
}

//获得的序列化结果
"A"
```

### 3.解决

#### 1.将一个属性作为给枚举序列化结果

通过在**属性/get方法**上添加注解` @JsonValue`

```java
@AllArgsConstructor
@Getter
public enum Eunm2 {
    /**
     * 枚举常量
     */
    A("情况1",200),B("情况2",500);

    @JsonValue
    private String name;

    private Integer num;
}
//序列化结果
// "情况1"
```





##### 注意点：

- 只能在将一个属性确定为该枚举的序列化结果

  如果如下面代码所示：同时在两个属性上标记`@JsonValue`注解，则会程序异常，

  ```java
  import com.fasterxml.jackson.annotation.JsonValue;
  import lombok.AllArgsConstructor;
  import lombok.Getter;
  
  @AllArgsConstructor
  @Getter
  public enum Eunm2 {
      /**
       * 枚举常量
       */
      A("情况1",200),B("情况2",500);
  
      @JsonValue
      private String name;
  
      @JsonValue
      private Integer num;
  }
  ```

  **异常字段：**

  ![异常](https://gitee.com/jj603786014/imgBed/raw/master/imgs/20200726214159.png)

### 2.将所有属性进行序列化

通过在枚举类上添加`@JsonFormat(shape = JsonFormat.Shape.OBJECT)`注解

```java
import com.fasterxml.jackson.annotation.JsonFormat
import lombok.AllArgsConstructor;
import lombok.Getter;

@AllArgsConstructor
@Getter
@JsonFormat(shape = JsonFormat.Shape.OBJECT)
public enum Eunm2 {
    /**
     * 枚举常量
     */
    A("情况1",200),B("情况2",500);

    private String name;

    private Integer num;
}

//序列化结果
// {"name":"情况1","num":200}
```

#### 注意点：

- 不能和` @JsonValue`一起使用