---
theme: default
highlighter: shiki
transition: slide-left
title: variable
layout: cover
---

# Programming 101 - 1
# 变量和内存

徐延楷 a.k.a. Froster  
20级的老东西

-----
# 先来一段程序......
老黄历了

``` java
public class Test {
    public static void main (String[] args) {
        Scanner s = new Scanner(System.in);
        int a = s.nextInt();
        int b = s.nextInt(); 
        int c = 1;
        int result = a + b + c;
        System.out.println(result);
    }
}
```

这个程序计算：$f(a, b) = a + b + 1$，很简单。大伙上了正课应该都看得出来。

----- 

# 变量
a，b，c还有result是什么？

``` java
int c = 1;
int result = a + b + c;
```

先看一道初中物理题的一行：

*...*  
*由xxx得，*$x_1 = v_0 + at = 114m$*。所以，*$x' = x_0 + x_1 = 514m$。

这里出现的字母（比如$x_1$）就可以看作变量，即装有真实数据（114m）的容器，或者对真实数据的引用。

变量和数学里的未知数不太一样。我们写下方程$x + 1 = 2$时，求解的目标是x。  
写下$x_1 = v_0 + at = ...$时，求解的目标是$x_1$，右边的$v_0, a, t$都是**已知的**。

回到那两行代码。按上面的思路，在这两行代码执行结束后：
- 在代码里写下c就是在指代1。
- 在代码里写下result就是在指代$a + b + c$的和。

-----

# 数据类型
1是什么？

``` java
int c = 1;
```

还是和数学不同。计算机里的变量都是有类型的。

先从数字开始：
- short / int / long：16 / 32 / 64位整数。e.g. $c \in [-2^{31}, 2^{31}), c \in Z$
- float / double：32 / 64位浮点数（带小数点的）

做这种区分的原因：计算机资源有限，无法高效的对长数字进行存储和运算。稍后讲内存的时候具体说。

这些数字变量之间可以互相运算——精度低的会自动转换为精度高的，反之不行。
``` java
long d = 2;
long e = c + d; // ok
int f = c + d; // error
```

-----

# 数据类型
`'a'`是什么？

然后是不像数字的：
- char：8位整数（$c \in [-128, 127]$，但只用0-127的部分），代表一个ASCII字符。  
计算机里的所有字符都是用数字表示的。e.g. 小写字母`'a'`在ASCII编码里是数字`97`。  
（稍微提一句：我们管这种把不是数的东西用数字表示的方式叫**编码**。
- boolean：布尔类型，代表真或假。命名来源于一个数学家。
``` java
boolean b1, b2;
b1 = true;
b2 = false; // 只有这两种值
```

比起上面的数值类型，布尔类型不能做数学运算，但是能做逻辑运算（计算机靠这个做判断）。
- `b1 && b2`：逻辑与。b1, b2全都为true则结果为true，否则为false。
- `b1 || b2`：逻辑或。b1, b2有一个为true则结果为true，否则为false。
- `!b1`：非。b1为true则结果为false，为false则结果为true。

-----

# 数据类型
`"abcdef"`是什么？

上面的类型可以通过几种方式组合（我们管这个叫数据结构，先讲两种）：
- 数组：多种同一类型的数据按顺序排列。
``` java
int[] arr = new int[]{1, 2, 3}; // arr里有三个数
int b = arr[0]; // 取出arr的第0个元素（稍后讲为什么从0开始），放入b。b显然等于1。
arr[1] = 5; // 把arr的第1个元素设为5。arr现在是{1, 5, 3}。 
```
btw，字符串也是数组。它只不过是有自己名字的一串char罢了。
- 类：多种不同类型的数据放在一起，造出一个新的数据类型。
``` java
class Person {
  String name;
  int age;
}
```
人类的本质是一个字符串加一个int类型的数字。  
排好队的人的本质是`Person[]`。我的意思是自己造的类型也可以通过这一页的方式组合。

-----

# 数据的运算
你说得对，但是`3 / 2 == 1`。

还是先从数字开始：数字可以加减乘除，但是，我们的数字是有类型的。

``` java
int a = 3, b = 2;
double c = 2.0;
// a / b == 1;
// a / c == 1.5;
```

上文提到，类型之间可以互相运算——精度低的会自动转换为精度高的。同一精度下不做转换。  
由于int是**整数**，所以除法的小数部分会被截断。`int / double`时，c的精度更高，所以转换成两个double，小数部分还在。

-----

# 数据的运算
你说得对，但是`"3" + 2 == "32"`。

数字和boolean以外的类型就和数学运算没直接关系了。这些类型的运算可以是编程语言定义，也可以是你自己定义。

比如，对于字符串，加法是字符串拼接：
``` java
String s = 1 + 2 + "3" + "4"; // "334"
```
从左向右，`1 + 2 == 3`，`3 + "3" == "33"`，`"33" + "4" == "334"`。 

#-------------------------------下文施工中----------------------------

-----

如果它看起来像鸭子、游泳像鸭子、叫声像鸭子，那么它就是只鸭子。

# 数据的运算
我们在说一些概念而非具体的事物

一些类型是由操作和数据一起定义的，比如说集合。
- 存储同类型的多个数据（`{1, 2, 3}`，不能是`{1, 2, 'a'}`）
- 支持添加，删除，查询等操作。

类比：ph试纸
- 一张轻的条状物（类型）
- 把溶液滴在上面可以通过变化来判断是酸还是碱（操作）

符合上述两条的都是ph试纸。
- 酸变红碱变蓝的石蕊试纸是ph试纸
- 酸变橙碱变紫红的酚酞泡过的纸条也是ph试纸
- 假设有一种金属，酸反应一个现象碱反应一个现象，那也可以是ph试纸（确信

-----