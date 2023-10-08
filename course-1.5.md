---
theme: default
highlighter: shiki
transition: slide-left
title: Brief chapter 0
layout: cover
---

# Java互助课堂（周一
# 1.5.简单过一下正课的Chapter 0

徐延楷 a.k.a. Froster  
20级的老东西

我怕Introduction时长不够所以来个1.5

----- 

# 编译
背诵内容

电脑本身没有执行`int a = b + c`的功能，只有执行`add r0, r1, r2`的功能

后面那是汇编语言，又臭又长又容易出错

所以得把你写的简单的人类能懂的java编译(compile)过去，干这活的叫编译器(compiler)。

编译器一次编译一整个程序，不太灵活。所以就有了解释器，一行一行编译。

-----

# JRE and JVM and JDK
背诵内容

JVM：一个**虚拟机(Virtual Machine)**。安卓模拟器也是个“虚拟机”

为什么要jvm：
- 现在有一个中国人和一个美国人，你想让他们算1+1
- 和中国人你得说“一加一”，和美国人你得说“one plus one”
- 那如果有一个印度人你还得说印度话...
- jvm起一个翻译软件的作用。你只需要写下1+1，交给jvm翻译一下，你就可以指挥全世界的人算1+1了

JRE：JVM + 一堆库。你有了电脑还要在上面装各种软件，JVM是电脑，JRE是电脑+软件

JDK：JRE + 开发工具。

java为啥又编译又解释：把你说的1+1**编译**成jvm能看懂的形式（字节码byte code），再根据平台**解释**成机器码

----- 

# lecture课件里的第一个程序
``` java
public class Welcome {
  public static void main(String[] args) {
    // comment
    /* 
    comment2
    */
    System.out.println("Welcome to Java Programming!");
  }
}
```

- `public, class, static`：后面再讲，在你们写多个文件的程序之前抄上去就好
- `public static void main(String[] args)`：主方法。计算机会从这个方法开始执行程序。
- `System.out.println("Welcome to Java Programming!");`语句。
- `//, /* */`：注释。给人看的。后面会讲怎么写一个自己过了一周还能看得懂的程序。
- `{}`：block，或者我更喜欢叫作用域。后面会讲。

-----

# 用命令行跑java程序
真有人记得住这个？

`javac test.java`编译

`java test`运行

考试顶多一道选择。记得带c的是编译(**C**ompile)，不带的就是运行。

-----

# 输入输出
你们学了class才会知道前面一大坨是干什么的...

## 输出
- `System.out.print(String s)`：把s打印出来
- `System.out.println(String s)`：把s打印出来再换个行。ln -> line
- `System.out.printf(String format, ...)`：这个可以格式化输出，非常的牛

``` java
int a; float b; String c;
System.out.printf("output integer: %d, float: %f, str: %s", a, b, c);
```

输出的时候会把abc从前往后填进`%d, %f, %s`里。百分号后面跟一个字母代表占位符。

`%d`是`int`类型，`%f`是`float`，`%s`是字符串。常用的就这仨，如果要输出`long`，或者要设定小数位数，整数前面补0什么的，自己查

类型错了会报`java.util.IllegalFormatConversionException: d != java.lang.String`

-----

# 输入输出
你们学了class才会知道前面一大坨是干什么的...

## 输入
``` java
// 程序的最前面：
import java.util.Scanner;

// ...

Scanner scanner = new Scanner(System.in); 
String s = scanner.next();
int i = scanner.nextInt();
T t = Scanner.nextT(); // T for any
```

没啥好说的，复制粘贴。你想要什么类型就next什么东西一下。

Scanner以空格或者换行作为一次读入的结束。

有一个`scanner.nextLine();`。印象里oj用这个会有bug，少用。

-----
# 转义
\

java的字符串在代码里只能写在一行上。所以我们得有个法表示换行...`\n`。

记得`\n`是换行，`\\`是"\"，`\"`是'"'就行。