# java中的位操作

## 1. 单独某一位取反其他不变

```java
//1
byte num = guaTis[0];// 要取反的数
byte mask = 0B1;//确定取反位置
// 保存除取反位外的其它位// 所有位取反，也就是异或;
num = (byte) ((num & (~mask)) | (num ^ mask));
```

