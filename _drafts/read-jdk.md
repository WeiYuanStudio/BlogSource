---
title: 啃JDK源码笔记之 Long
categories:
  - 笔记
  - Java
tags:
  - Java
  - 笔记
  - 源码
date: 2020-05-3 02:10:00
---

开一个大坑，啃JDK源码，感觉这都可以出一个系列了。

参照CodeSheep@Bilibili大佬的视频[BV1V7411U78L](https://www.bilibili.com/video/BV1V7411U78L)，找一些经典来啃

 - io
 - lang
 - math
 - net
 - nio
 - time
 - util

正好今晚看到Long，这篇稿子就从Long开始吧

## Long

### parse

方法签名

```java
public static long parseLong(String s, int radix)
```

单参重载签名如下，十进制默认就用这个

```java
public static long parseLong(String s)
```

很简单，单参直接调用双参方法，然后radix赋10

```java
public static long parseLong(String s) throws NumberFormatException {
    return parseLong(s, 10);
}
```

现在直接来看双参这个方法吧，

首先上来第一步就是检查是否null，这里如果null不是抛NPE(NullPointerException)，而是抛

```java
throw new NumberFormatException("null");
```

例程

```java
import java.lang.Long;

public class Main {
    public static void main(String args[]) {
        Long i = Long.parseLong(null);
    }
}
```

输出

```text
➜  play-ground java Main 
Exception in thread "main" java.lang.NumberFormatException: null
        at java.base/java.lang.Long.parseLong(Long.java:655)
        at java.base/java.lang.Long.parseLong(Long.java:817)
        at Main.main(Main.java:5)
```

所以遇到那种可能会向里面传null的情况，建议在传入`parseLong()`前套一个`Objects.requireNonNull()`这样遇到null会直接抛NPE，方便catch。

接着会检查`radix`取值范围`2(Character.MIN_RADIX)-36(Character.MAX_RADIX)`

为什么`Character.MAX_RADIX`为`int 36`呢，一开始我也没想懂这个问题，看起来有点眼熟，但是不明原因，到后面我看到`Character`的`DIGITS`数组恍然大悟，`36 = 10（十进制范围，0-9数字） + 26 （26个英文字母）`超出这个范围的话，英文字符也不够表示这么一个数了。

初始化几个值

```java
boolean negative = false; //默认这个数是正数
int i = 0, len = s.length(); //i储存读到第n位char，len就是参数string的长度
long limit = -Long.MAX_VALUE; //默认正数，所以也设定最大值不得大于//Todo
```

检查string长度，不得小于0

判断第一个字符，如果字符小于`'0'`的话，判断正负号，这里用命令看了下`man ascii`的确字符`+ -`都在`0`的前面。很巧妙的一个做法，一步就判断了是否为符号

```java
if (firstChar < '0') { // Possible leading "+" or "-"
    if (firstChar == '-') { //如果为负数，再将前面的默认初始化修改
        negative = true;
        limit = Long.MIN_VALUE;
    } else if (firstChar != '+') { //如果不是正号直接抛出
        throw NumberFormatException.forInputString(s);
    }

    if (len == 1) { // Cannot have lone "+" or "-"
        throw NumberFormatException.forInputString(s);
    }
    i++;
}
```

```java
long multmin = limit / radix;
long result = 0;
while (i < len) {
    // Accumulating negatively avoids surprises near MAX_VALUE
    //产生负数可避免在MAX_VALUE附近出现意外，负数的取值范围更广
    int digit = Character.digit(s.charAt(i++),radix);
    if (digit < 0 || result < multmin) {
        throw NumberFormatException.forInputString(s);
    }
    result *= radix;
    if (result < limit + digit) {
        throw NumberFormatException.forInputString(s);
    }
    result -= digit;
}
return negative ? result : -result; //如果是正数到最后一步再进行反转
```

这里要将字符通过`Character.digit(s.charAt(i++),radix);`先转成十进制的int，然后再回来计算。在Character那边会有一个`int digit(int ch, int radix)`以及DIGITS数组将字符`0-9` `A-Z` `a-z`映射到`0-35`的int范围上返回。详细的可以看`CharacterDataLatin1.java`部分

`int digit(int ch, int radix)`函数以及DIGITS数组将字符`0-9` `A-Z` `a-z`映射到`0-35`的int范围上返回

```java
int digit(int ch, int radix) {
    int value = DIGITS[ch];
    return (value >= 0 && value < radix && radix >= Character.MIN_RADIX
            && radix <= Character.MAX_RADIX) ? value : -1;
}
```

```java
// Digit values for codePoints in the 0-255 range. Contents generated using:
// for (char i = 0; i < 256; i++) {
//     int v = -1;
//     if (i >= '0' && i <= '9') { v = i - '0'; } 
//     else if (i >= 'A' && i <= 'Z') { v = i - 'A' + 10; }
//     else if (i >= 'a' && i <= 'z') { v = i - 'a' + 10; }
//     if (i % 20 == 0) System.out.println();
//     System.out.printf("%2d, ", v);
// }
//
// Analysis has shown that generating the whole array allows the JIT to generate
// better code compared to a slimmed down array, such as one cutting off after 'z'
// 大概意思是写死整个数组可以使JIT生成性能更好的代码，空间换时间
private static final byte[] DIGITS = new byte[] {
    -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,
    -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,
    -1, -1, -1, -1, -1, -1, -1, -1,  0,  1,  2,  3,  4,  5,  6,  7,  8,  9, -1, -1,
    -1, -1, -1, -1, -1, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24,
    25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, -1, -1, -1, -1, -1, -1, 10, 11, 12,
    13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32,
    33, 34, 35, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,
    -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,
    -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,
    -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,
    -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,
    -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1,
    -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1 };
```

经过查ascii表decimal十进制可知，上数组中`0-9`分别对应数字字符中的`0-9`，后面隔开7个`-1`分别是对应ascii中的7个符号字符，接着从`10-35`分别对应`A-Z`和`a-z`字符。

这里可以将比如16进制这样超出`0-9`十进制范围的数字字符映射到十进制对应的`10-35`

### decode

该方法封装了`valueOf(String nm, int radix)`方法，通过判断开头的`+ -`以及`0X 0x #`来判断是否正负以及16还是10进制

### valueOf

```java
public static Long valueOf(long l) {
    final int offset = 128;
    if (l >= -128 && l <= 127) { // will cache
        return LongCache.cache[(int)l + offset];
    }
    return new Long(l);
}
```

这里可以看到，当属于`l >= -128 && l <= 127`的long值，会去静态类`LongCache.cache`里面调取。**如果超过该范围，就会新建一个Long对象。**

```java
private static class LongCache {
    private LongCache(){}

    static final Long cache[] = new Long[-(-128) + 127 + 1];

    static {
        for(int i = 0; i < cache.length; i++)
            cache[i] = new Long(i - 128);
    }
}
```

可以看到这个类在首次加载的时候就会被初始化，将创建范围`l >= -128 && l <= 127`的long对象缓存。

回到上面重点，**如果超过该范围，就会新建一个Long对象。**意味着，如果你使用`==`运算符去比较Long的**包装类型**，将会造成意料之外的问题。

在范围`l >= -128 && l <= 127`还好说，会调用初始化时创建好的缓存对象，刚好返回的是缓存，内存地址相同。但是超出这个范围的**包装类型**，进行`==`运算的时候就遭殃了，`valueOf`将创建一个新的对象返回。他们两个的地址将不可能为同一内存地址。

贴测试代码

```java
import java.lang.Long;

public class Main {
    public static void main(String args[]) {
        System.out.println("====> Use ==");
        for (Integer i = -129; i <= 128; ++i) {
            String valueString = i.toString();
            Long j = Long.parseLong(valueString);
            Long j2 = Long.parseLong(valueString);
            if (j != j2)
                System.out.println("!= : " + i);
        }
        System.out.println("====> Use .equals");
        for (Integer i = -129; i <= 128; ++i) {
            String valueString = i.toString();
            Long j = Long.parseLong(valueString);
            Long j2 = Long.parseLong(valueString);
            if (!j.equals(j2))
                System.out.println("!= : " + i);
        }
        System.out.println("====> long");
        for (Integer i = -129; i <= 128; ++i) {
            String valueString = i.toString();
            long j = Long.parseLong(valueString);
            long j2 = Long.parseLong(valueString);
            if (j != j2)
                System.out.println("!= : " + i);
        }
    }
}
```

输出：

```text
➜  play-ground java Main      
====> Use ==
!= : -129
!= : 128
====> Use .equals
====> long
```

所以，在**对比两个long的包装类型是否相等**的时候，请务必使用`equals()`方法。否则遇到范围`l >= -128 && l <= 127`之外的long值，进行`==`运算时将发生意料之外的返回。

其次，创建Long对象时，请不要使用`public Long()`构造，而是使用`valueOf()`因为当数值属于`l >= -128 && l <= 127`时，会优先返回内部静态类内的缓存对象，而不是直接调用构造创建新对象，从而优化内存使用。

PS: `Integer`里面也差不多，不过`Integer`内提供了可更改的默认初始化范围，下限参数`-128～`，写死。启动jvm时参数`-Djava.lang.Integer.IntegerCache.high=<highValue>`参数可以设置缓存上限，上限未写死，默认值127，设置值不得低于127，低于127设置无效。`Integer`能修改上限是特例，改天看到`Integer`部分再详细贴这部分代码

### toString

Long的`toSting()`

```java
public static String toString(long i, int radix)
```

首先检查`radix`，如果不在取值范围`2(Character.MIN_RADIX)-36(Character.MAX_RADIX)`，直接设置默认`radix = 10`不抛出

接着判断是否`radix == 10`，若真返回`toString(i)`，即移交专门处理十进制数的方法。**该特例会在下面讲**

若非`radix == 10`将在下面进一步处理

这里发现一个有趣的设置，会先进行`java.lang.String.COMPACT_STRINGS`的判断，默认值true，检查是否开启字符压缩，关于这个设定的详细信息可以去看`java.lang.String`，里面有详细的介绍,若关闭压缩，则默认使用`LATIN1`编码，否则会使用`UTF16`编码

如果使用`UTF-8`编码的话，会在当前方法中进行`toString()`，否则会移交`toStringUTF16()`处理

当前方法处理`UTF-8`编码，

```java
byte[] buf = new byte[65]; //先建立一个byte类型的64长度数组作为buffer
int charPos = 64; //当前写入字符的位置
boolean negative = (i < 0); //是否负数，若真，会在写入完所有数字之后，在最前面加入负号
```

接着下面就是对要处理的long i 值不断

```java
i % radix //对进制radix取余
i = i / radix; //自除以进制radix
```

的操作，得到的`int`值会到`Integer.digits`数组中找对应的字符

```java
static final char[] digits = {
    '0' , '1' , '2' , '3' , '4' , '5' ,
    '6' , '7' , '8' , '9' , 'a' , 'b' ,
    'c' , 'd' , 'e' , 'f' , 'g' , 'h' ,
    'i' , 'j' , 'k' , 'l' , 'm' , 'n' ,
    'o' , 'p' , 'q' , 'r' , 's' , 't' ,
    'u' , 'v' , 'w' , 'x' , 'y' , 'z'
};
```

这里奇怪的是这么一个`static final`值的命名方式居然不是全大写风格，猜测是早期的历史遗留问题，我倒是特意翻了下`Integer`的所有`static final`域，发现风格不一（全小写，首字大写驼峰）的基本上都是1.5之前的代码。

总体来说，`toString`方法有几点不同的
- 处理是否压缩字符的不同
    如果是处理非压缩字符使用`UTF16`的话，无论是10进制还是其他进制，初始化`buffer`的时候，容量是压缩字符`LATIN1`编码的两倍
- 处理10进制数和非10进制数的不同
    - 首先是创建buffer大小的不同
        十进制数创建buffer的时候会计算`stringSize()`，其他指定非10进制的`toString`方法会直接上来分配`byte[65]`
    - 其次是处理算法上的不同
        在处理十进制数的时候，一次自除100，一次处理两位，且用加减计算代替取余计算以优化性能。余数显然范围在`0-99`，所以取字符操作会查找`java.lang.Integer.DigitOnes`与`java.lang.Integer.DigitTens`这两个长数组，空间换时间。

```java
static final byte[] DigitTens = {
    '0', '0', '0', '0', '0', '0', '0', '0', '0', '0',
    '1', '1', '1', '1', '1', '1', '1', '1', '1', '1',
    '2', '2', '2', '2', '2', '2', '2', '2', '2', '2',
    '3', '3', '3', '3', '3', '3', '3', '3', '3', '3',
    '4', '4', '4', '4', '4', '4', '4', '4', '4', '4',
    '5', '5', '5', '5', '5', '5', '5', '5', '5', '5',
    '6', '6', '6', '6', '6', '6', '6', '6', '6', '6',
    '7', '7', '7', '7', '7', '7', '7', '7', '7', '7',
    '8', '8', '8', '8', '8', '8', '8', '8', '8', '8',
    '9', '9', '9', '9', '9', '9', '9', '9', '9', '9',
    } ;

static final byte[] DigitOnes = {
    '0', '1', '2', '3', '4', '5', '6', '7', '8', '9',
    '0', '1', '2', '3', '4', '5', '6', '7', '8', '9',
    '0', '1', '2', '3', '4', '5', '6', '7', '8', '9',
    '0', '1', '2', '3', '4', '5', '6', '7', '8', '9',
    '0', '1', '2', '3', '4', '5', '6', '7', '8', '9',
    '0', '1', '2', '3', '4', '5', '6', '7', '8', '9',
    '0', '1', '2', '3', '4', '5', '6', '7', '8', '9',
    '0', '1', '2', '3', '4', '5', '6', '7', '8', '9',
    '0', '1', '2', '3', '4', '5', '6', '7', '8', '9',
    '0', '1', '2', '3', '4', '5', '6', '7', '8', '9',
    } ;
```

在超出int范围的循环自除得出每一位字符这种操作，会使用long来计算，当处理到i落到int范围时，会做一个显示类型转换，将余下的long转为int，使用int继续完成接下来数字的处理，优化性能。

在最后阶段，`i2 <= -100`，剩余数据只剩两位了，就会除以10，对10取余，处理最后的两位。而且在最后两位的转字符处理不再去字符数组里面查找，而是直接对`'0'`字符进行加操作得出目标字符的字符值。在处理完毕数字字符后，最后处理加入负号`-`，返回。
