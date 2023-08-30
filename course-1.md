---
theme: default
highlighter: shiki
transition: slide-left
title: Welcome to Slidev
layout: cover
---

# Programming 101 - 0 

徐延楷 a.k.a. Froster  
20级的老东西

----- 

# **我们**为什么要写代码？

+ 计算
  - 求解 $f(x) = ax + b$
  - 求解 $f(x) = 1145f(x^{14}) + 1919x - \frac{810}{x}$
  - 求解一张图像上有没有人脸
  - 求解从宝安机场到南科大怎么走最近
  - 现有一张excel表，过滤掉里面所有姓名不为张三的人
+ 控制
  - 让一盏led灯亮起来
  - 让一盏led灯每秒闪烁一次
  - 让一盏led灯以1, 1, 4, 5, 1, 4秒的间隔循环闪烁
  - 现有一辆往前跑的小车，如果车前面有障碍物则右转继续跑
  - 让一个游戏人物的模型在按下鼠标左键时做铁山靠

----- 
# 功能的组合
如何实现上面的需求？

<!--要实现上面的功能，需要知道计算机都能干啥-->

<div grid="~ cols-2 gap-4">
<div>

+ 求解 $f(x) = 114x + 514$
  - 输入 $x$ （从设备，比如键盘，输入）
  - 计算乘法：$114 \times x$，**保存结果**，记作 $a$
  - 计算加法：$514 + a$，保存结果，记作 $b$
  - 输出 $b$ （显示到某个地方）

$$
  114 * 10 = 1140 \\
  1140 + 514 = 1654
$$
</div>
<div>

``` java
f() {
    // print(114 * readNumber(x) + 514);
    int x = readNumber(x);
    int a = 114 * x;
    int b = a + 514;
    print(b);
}

```
</div>
</div>

-----

# 功能的组合

<div grid="~ cols-2 gap-4">
<div>

+ 有一辆小车，让它持续前进，如果车前面有障碍物则右转再继续前进
  - 控制小车的轮子往前滚
  - 读取传感器数据
  - 如果：
    + 传感器数据显示有障碍：
      - 控制轮子停下
      - 右转
    + 传感器数据显示没有障碍：
      - 什么都不做
  - **回到第一步**

</div>
<div>

``` java
control() {
    while (true) {
        moveForward();
        SensorData data = readSensorData();
        if (hasObstacle(data)) {
            stopMoving();
            turnRight();
        } else {
            // do nothing
        }
    }
}
```
</div>
</div>

-----

# 功能的组合
编程的本质其一

rt，组合已有的功能。

最类似的可能是化学大题里写有机合成路线。

对已有功能的记忆+熟悉常见的组合模式。

-----

# 功能从哪来？
layers of abstraction

<!--有点像公理-->
计算机提供：加减乘除，判断，跳转，存储......

编程语言和第三方库提供：乘方，对数，求导，读写文件，访问网络，控制外设......

编程的绝大部分工作是在发明自己的功能（我们把这个叫函数/方法）。

鲁迅没有说过：编程就是把问题不断分解为小问题，小问题分解为更小的问题，最终分解到已有的功能上去。

``` java
void anotherControl() {
    boolean flag = isButtonPressed();
    if (flag) {
        control();
    }
}

boolean isButtonPressed() {
    ...
}
```

-----

# 来点练习？
regexp!

## 正则表达式

一个匹配文本的工具。虽然不能像编程语言一样做很多事，但在查找和替换文本上极其有用。

1. 准备工作

- 打开你的idea/vscode/pycharm/clion....，如果你没有ide，打开https://regex101.com/
- 对于使用ide的同学，打开查找和替换功能（一般是ctrl+r）
- 在上方的查找框最右侧有一个`.*`图标。点击进入正则表达式搜索模式。
- 准备一段文本（这个之后我准备）

-----

# 基础语法
省略版

普通字符：
- 普通字母数字和符号：匹配普通字母数字和符号。和平时的搜索功能一样，输入`aa1`可搜索出所有"aa1"。不是所有的符号都能直接匹配（就像下面的`.`被占用了），但练习中不会涉及到没讲过的不可匹配的符号。
- `.`：匹配任意字符。
- `\w`：匹配单个字母
- `\d`：匹配单个数字
- `\n`：匹配换行符

-----

# 基础语法
省略版

特殊字符（对普通字符的修饰）：
- `*`：尽可能多次（0次也行）的匹配`*`前面的单个字符。比如，`ca*bx`可以匹配"cbx"，"cabx"，"caaaaaaaabx"。`c.*b`匹配"cb"，"caaaaabaaab"，"c114514e5b"。
- `?`：
  + 如果前面不为`*`，匹配前面的字符0或1次。比如，`ca?b`匹配"cab"和"cb"，`c.?b`匹配"cb"，"c1b"......
  + 如果前面为`*`，则更改`*`的行为为尽可能少的匹配`*`前面的单个字符。对于"caaaaabaaab"，`c.*b`匹配整个字符串，而`c.*?b`匹配前半段"caaaaab"（遇到第一个b就停了）。`.*?`是一个很实用的组合。
- `[]`：匹配任意中括号内的字符。如`[ab]`匹配a或b。`[\w\n]`匹配单个字母或换行符。中括号可以组合其他特殊字符使用，如`[ab]*`匹配任意多的a或b。

其余语法不会涵盖在本次练习内。有兴趣的同学可以自己查找资料学习。

-----

# 练习
~~现在你已经对算术的基本原理有了一定了解...~~

- 匹配一个任意字母，后面跟一个任意数字的组合
- 匹配一个0-99闭区间内的数字。"09"和"9"都应该匹配。
- 匹配所有开头为a的行。第一行保证为空行。（bonus：如果第一行不为空行怎么办？这个需要上网查，是课件里没教的语法）
- 匹配一个网址的origin。一个网址的origin由`http://`或`https://`开头，中间为`a.b.c`的格式，可以有一个`/`作为结尾。例如，"http://www.baidu.com"或"https://a.b.c/"。
- 匹配所有以public abstract开头的函数。需要匹配大括号内的完整函数体。提示：用`[\w\n\d ]`而非`.`匹配函数体内的内容。后者不会匹配换行符和空格。

-----

# 一些编程的本质
我自己说的，仅供参考

上文提到，鲁迅没有说过：编程就是把问题不断分解为小问题，小问题分解为更小的问题，最终分解到已有的功能上去。

这句话里的“问题”是一开始所讲的——针对**数据**的计算和控制。

把输入的数据经过你定义的过程处理成你想要的数据。比如说，计算一系列数的最大值，或者把字符串里的单词全部转为大写。

或者，根据输入的数据，经过你定义的过程去做一些事情。比如说控制机器人，从网上下载指定文件。

编程会让你学会如何定义过程，也就是教计算机做事。但是“输入的数据”和“你想要的数据”，即问题本身，比定义过程更重要。

写代码的时候永远要知道自己的目标。

-----

# 如何学习
一点人生的经验（被枪毙

First of all.....
### 我不是寄系的，可以混过去吗？
可以。但是编程以后是你吃饭的家伙之一。你可以把高等数学忘光光，但进了实验室多少还得写点代码。

-----

# 如何学习
a.k.a. 我们学编程的时候是在学什么？

上文两次提到，鲁迅没有说过：编程就是把问题不断分解为小问题，小问题分解为更小的问题，最终分解到已有的功能上去。

写代码的能力 = 用“编程思维”描述问题 + 知道问题如何分解 + 知道已有的功能 + 知道如何组合已有的功能

~~非常不负责的~~拿去年的lab题目举个例子：

给定$p, q, a_0...a_n$，求解$\int_{q}^{p} a_0 + a_1x + a_2x^2 + ... + a_nx^n$。

如果是在数学卷子上看到，步骤应该是这样：

$\int a_0 + a_1x + a_2x^2 + ... + a_nx^n = a_0x + \frac{a_1x^2}{2} + ... + C$，然后代数，做减法。

-----

# 如何学习
a.k.a. 我们学编程的时候是在学什么？

现在的问题是，如何计算$a_0x + \frac{a_1x^2}{2} + ... + C$？（问题）

要计算上面的式子，需要：让程序知道$n, a_0...a_n$，计算$x^i, a(x^i), a_0x+\frac{a1x^i}{2}+...$，即算乘方，算加法乘法除法，算n个数的加法，再把结果打印出来。（小问题）

如果你们上了第一节lab，应该已经知道怎么输入输出了。刚才也提过计算机可以算加减乘除。（已有的功能）

剩下的问题是，怎么算n个数的加法和乘方。

过几节课你们会学到循环结构，也就是让计算机把一件事干n遍。用这种结构可以算n个数的加法。（还是已有的功能）

对于乘方，我们知道乘方是做n次乘法，所以可以像上面算n个数的加法那样自己写一个乘方的函数（功能），也可以**进行一个搜索引擎的使用**，发现有一个函数（比如`math.pow()`）是算乘方的，直接利用前人写好的东西。

不像数学公式，前人写好的东西太多了，没有人记得住。大火都是有需要的时候现查的。

-----

# 如何学习
a.k.a. 我们学编程的时候是在学什么？

最后动用一下你的逻辑思维把这些东西全都组合起来（这一块就是 知道一些范式，然后多练！）：

+ 读入所有的a，以及p, q
+ 记那个公式是g(x)，问题的答案是g(p) - g(q)
  - g(x)是n个数的求和，要求和必须先知道数等于什么。用循环结构依次计算累加
    + 第i个数是$\frac{a_ix^i}{i+1}$，计算之
    + 把第i个数和前面的累加结果加起来
+ 输出结果

``` java
  // 伪代码
  a_arr, p, q = ... // a = [1, 2, ...], p = 114, q = 514 读入环节
  // g(p), g(q)
  sum_p = 0, sum_q = 0
  for (i = 0; i < a_arr.length; i++) { // 循环，你们现在只需要知道这个里面的两行执行n次，第几次由i表示
    sum_p = sum_p + (a_arr[i] * math.pow(p, i)) / (i + 1)
    sum_q = sum_q + (a_arr[i] * math.pow(q, i)) / (i + 1)
  }
  print(sum(p) - sum(q)) // 输出环节
```

-----

# 我的代码为什么报错了？
又名，提问的智慧

首先，虽然**报错信息**它是英文的，而且里面估计有114514个你没见过的词，但是它设计出来

## 是为了给人看懂的！！！！！！！！

错误的处理方法：报错->大佬这里怎么改->大佬这里怎么改->大佬这里怎么改->大佬这里怎么改->... 

然后不进脑子，下次还错，然后**反复**用一些简单的问题折磨sa。

正确的处理方法：报错->**为什么错？**
- 报的这个错没见过->它是什么？->学会->以后不犯错，或者学会解决相似问题的方法
- 没报错，但是oj说你错了/你发现结果不对：利用debug工具或者打印查看中间结果->找到逻辑漏洞->结束

-----

# 怎么问问题？
什么问题合适？

虽然自主解决问题的能力必须有，但是提问也是必须的！SA，TA和教授们很喜欢你们提问！

~~SA不喜欢也得喜欢，人每个月是领工资的，不用担心麻烦人~~

提问的智慧：https://github.com/tvvocold/How-To-Ask-Questions-The-Smart-Way

建议所有人回去通读一遍。省流版本：
- 以下所有的都是态度问题。不要把其他人当成保姆:(
- 问之前先用搜索引擎查一下，SA不是你的浏览器（推荐使用Google，其次bing，百度狗都不用）
- 问之前简单翻翻聊天记录。
- 问之前先根据你的代码和报错自己解决一下问题。如果没有成功，问的时候附带上你的解决方法。
- 问的时候提供报错和代码。如果无法提供代码（怕查重），说思路，或者小窗问sa。不提供这些内容的问题就像“我今年高考数学倒数第二道大题错了，为什么？”
- 问的时候带上你的脑子。你得到的会是思路和解决方法，而非具体的代码。
- 问之前自己试一下。形如“a这样用会不会报错”这种问题，很多时候是自己敲进去跑一下就知道的事情

-----

# 老师没教怎么办？
啥都不会，一看就是上大学上的 ~~（不是原神）~~

## **自己学。**

大学，至少是计算机课程，没有超纲。

别人写作业写仨小时，你用老师没教的牛逼方法三分钟把作业秒了，那你牛逼，活该多玩三小时。

（老师教的你也得会，不过编程会了后面的前面的也应该会了。）

而且，知识太多了，教授又不是专门讲课的老师，不可能什么都讲。

讲计算机的书本来就没有其他学科那么成体系，而且越到后面越少（虽然足够cover计算机系课程了），到最后你只能去读官方文档，甚至原代码和论文。

b站和ytb上不少很好的课，大伙可以跟着学学。

-----

# 科学上网
远离csdn

几乎是必备的。你需要流畅的访问Google，StackOverflow，Github，Chatgpt，各种乱七八糟的官网，以及使用各种包管理器。

搜索引擎排名：谷歌大于bing。不要百度。

使用这两个搜索引擎的国际版，然后用英文关键词搜索。这样你能搜到StackOverflow的答案，能解决99%的问题。

中文社区真的不行，少看csdn。StackOverflow上的回答一般会仔细分析问题，给出多种解法，还有很多人的讨论。csdn上一般只贴一种解决方法，还不一定好使。

-----

# 如何看待LLM？
天坑

个人不建议在第一门编程课上使用ChatGPT，一点都不要碰。
- 写代码是需要大量练习的。用一学期chatgpt等于高中抄一学期数学作业，到头啥也做不出来。
- 使用搜索引擎和向别人提问是很有用的技能。有的东西chatgpt没法告诉你，前两者可以，但是也需要一点点练习。
- 它不总是对。可能你写代码也就半小时，但是问chatgpt五分钟，debug两小时。
- 而且你现在没有判断他对不对的能力。
- LLM没有设计代码的能力。它可以很好的写一个函数，但遇到几千行规模的代码无能为力。

这玩意有点像搜题软件。不过软件能保持正确率，它不能。而且用过搜题软件的大伙应该都明白，需要自制力...

-----

# 预告

- 正则表达式
- 类型系统，变量，内存
- 下节课之前你们java课上讲过的东西