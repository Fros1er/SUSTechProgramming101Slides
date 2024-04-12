---
theme: default
highlighter: shiki
transition: slide-left
title: variable
layout: cover
---

# Java互助课堂（周一
# 4. 面向对象

徐延楷 a.k.a. Froster  
20级的老东西

抽象，继续抽象

-----
# 为啥要面向这个对象。。。
其实是面向数据和类型

上次课的这个例子：这个方法要返回一个二维的点。

``` java
int[] getNewPosByChar(char command, int x, int y) {
  switch(command) {
    case 'q':
      return new int[]{x - 1, y - 1};
    ...
  }
}
```

我们在这里用一个`int[]`代表点。这时候就出现了一个问题：这玩意不是数组吗？？？

数组是用来表示一堆**不定长**的数的，在这里表示点不合适。

而且这个数组的第0项是x还是第1项是x，这个点是二维的还是三维的，这个返回值完全看不出来。

-----
# 为啥要面向这个对象。。。
其实是面向数据和类型

第二节课讲类型的时候有提到：

```
char是字符，short是数。

当我们看到一个变量的时候，它的类型会给我们一些额外的信息，比如：
- 它里面存储什么数据？
- 它在这里是被用来干什么的？
- 它能支持什么操作？
```

`int[]`在这个场景给不出什么信息。 很自然的，我们在这里需要一个新类型，代表：
- 这个类型里面存储int类型的坐标`x`，`y`。
- 这个类型用来表示一个二维的点
- 没啥操作，我只想表示一个点

基本类型和数组干不了这个事情。该**创建新数据类型了**。后文会把这些数据类型统称为“类”(class)。

-----
# class：数据类型的定义
这时候还没到OOP，我在讲class的先人——struct

``` java
public class Point2D {
    int x, y;
}

Point2D getNewPosByChar(char command, Point2D currPos) { /* ... */ }
```

清楚多了。我们还可以：
``` java
Point2D[] points = new Point2D[10];
// int[][] points = new int[][10];
```

同样，`int[][]`是二维数组，而非一堆点。

我们还可以：
``` java
currPos.x; // currPos[0]
```

一眼就知道`currPos.x`是x坐标，但`[0]`就看不出来。

-----
# 数据的聚合
点是x, y两个int，其他东西呢？

``` java
public class Point2D {
    int x, y;
}
```
可以看到Point2D里有两个成员变量(instance variables, member, field, ...)，说明一个点由两个数组成。

假设要定义一个棋类游戏的游戏玩家类。一个抽象的正在下棋的游戏玩家由什么“数据”组成呢？

- 昵称：`String`
- 先手还是后手：`boolean`，代表是否为先手
- 此前的胜场：`int`
- 是否为AI：`boolean`
- ......

把这些数据**统一管理**起来，就是我们的`Player`类。

-----
# 还是数据的聚合
class套class

一个复杂的项目会由很多类组成...

玩家定义完了，该棋盘了。棋盘这东西简单，一堆格子，格子里面能装棋子`Piece`。

这时候就要看需求了——
- 你在下国际象棋。格子就是格子，除了放棋子没有其他用处。
    + 二维数组就是个很好的格子。数组的每一项要么有东西要么没有。
    + `Piece[][]`。格子里面要么有棋子，要么没有（即`null`）。
    + 棋子的颜色和类型怎么看？棋盘就是个数组而已，问里面装的棋子（即`pieces[0][0].color`）。
- 你在下斗兽棋。格子分陆地河流陷阱还有大本营。
    + `Piece[][]`不够用了。这个只能表示某个位置有没有棋子。
    + 来个Grid类。然后`Grid[][]`。`Grid.type`表示格子类型，`Grid.piece`表示格子里面的棋子（可为`null`）。

-----
# 还是数据的聚合
class套class

全写下来，就是：

``` java
class Piece { /* ... */ }

class Grid {
    int type; // bad，正确写法暂时超纲了
    Piece piece; // 可以为null
}

class Board {
    // Piece[][] pieces;
    Grid[][] grids;
}
```

注意：grids里的每一项都不能为`null`。棋盘怎么能缺格子呢？

但是piece可以，格子上没棋子太正常了。

写代码的时候脑子里要有这个意识...关于什么能是null什么不能是。如果不能是，最好写个注释。

-----
# 实例
int和int x的区别

``` java
Point2D point;
```

我们刚才说的class是类型，和int一样。类型是个概念，不是实在的东西。

举个例子：
- 一个点的x坐标值是什么？不知道。我只知道一个点有xy，具体哪个点的坐标？
- `point`的x坐标值是什么？`point.x`。
- `point1`的x坐标值是什么？`point1.x`。

`class Point`里面写的`int x`的意思是“规定每一个point实例里都有一个叫x，类型为int的成员变量”，  
而不是像方法里写的`int x`，代表“创建一个int类型变量x”。它只是规定，实际得创建point实例的时候才会有

实例化(Instantiation)：把这个具体的东西创建出来。就是，`new`一个。

下面会称这个具体的东西为实例(Instance / Object)。

-----
# 构造器(constructor)
数据怎么装进去？

``` java
public class Point2D {
    int x, y;
    public Point2D(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

Point2D point = new Point2D(0, 1);
```

构造器/构造方法用来初始化一个实例。

比如说这个`Point2D`，一个点肯定要有它的xy坐标，所以我们写一个构造器，传入x，y两个值，然后把它放进成员变量里。

-----
# 构造器
初始化

``` java
public class Board {
    Grid[][] grids;
    public Board() {
        this.grids = new Grid[10][10];
        for (int i = 0; i < 10; i++) {
            for (int j = 0; j < 10; j++) {
                this.grids[i][j] = new Grid(i, j);
            }
        }
    }
}
```

再比如说这个Board。假定我们默认棋盘是10*10的。

每一个board被创建的时候，它的格子需要被初始化好（棋盘必须要有格子，不存在没有格子的棋盘）。

我们在构造器里做这个。确保每一个实例创建好之后不会有格子不存在的情况。

构造器的作用...初始化你操作这个实例所需要的一切东西。

-----
# 能不能没有constructor？
如能

有的人要问了：我能不能不用构造函数，new完之后在外面赋值？

<div grid="~ cols-2">
<div style="margin-right:2em;">

``` java
public class Board {
    Grid[][] grids;
    public Board() {}
}

static Board createBoard() {
    Board b = new Board();
    b.grids = new Grid[10][10];
    for (int i = 0; i < 10; i++) {
        for (int j = 0; j < 10; j++) {
            this.grids[i][j] = new Grid(i, j);
        }
    }
    return b;
}
```
</div>
<div>
能，但是你的队友没注意到。。。

``` java
Board loadGame() {
    Board b = new Board();
    for (int i = 0; i < 10; i++) {
        for (int j = 0; j < 10; j++) {
            this.grids[i][j] = getGridFromFile(i, j);
        }
    }
}
```

然后就NullPointerException了。
</div>
</div>

-----
# 能不能没有constructor？
<div />

有的人继续问：就一个空指针，改一下不就好了

你说得对，但是~~原神~~你的队友可能会这么改：
- 把`new Board()`改成`createBoard()`。最正常的，但是用构造方法不用写这一行，还不用经历一次报错
- 加一行`b.grids = new Grid[10][10];`。出现了野生的重复代码...你们之后改棋盘大小的时候多来一次报错
- 给Board加一个构造方法。出现了更多野生的重复代码！
- （想象力有限，肯定有更屎的改法）
- 发消息：“这里报错了你改一下”，然后爽打游戏，你回复之前绝不干活！

其实不止是constructor的问题...

在合作写代码（无论是你和队友还是你和一个月后的自己）的时候，小组/团队里需要很多约定，比如创建Board的时候需要`createBoard()`。但是大伙都是人，总有忘记或者没看到的时候。不遵守约定就会出bug。  
项目规模大一点的时候会写注释和文档，也有一些语言功能或设计模式来尽量减少约定的数量。  
使用构造函数的话就可以少这么一个约定，非常方便！

（我一直很喜欢提“你的队友”，毕竟java里的很多设计都是为了这个做的）

-----
# 很多构造器
<div />

有时候，一个类不一定需要传进去参数，还支持用默认值初始化 \
（比如你要画一个矩形，这个矩形默认是红色，但你可以指定其他颜色

``` java
public class Rectangle {
    Color color;
    public Rectangle() {
        this(Color.red);
    }
    public Rectangle(Color color) {
        this.color = color;
    }
}

Rectangle rect1 = new Rectangle(Color.blue);
Rectangle rect2 = new Rectangle();
```

重载（overload）一个构造器就行。调用哪个构造器取决于new的时候传什么参数。和重载方法一样。

这里的`this()`代表调用其他构造方法。只能在构造方法的第一行写。一般是参数少的构造方法里只写一行，只调用参数多的构造方法。


-----
# this
<div />

``` java
public class Point2D {
    int x, y, z;
    public Point2D(int x, int y) {
        this.x = x;
        this.y = y;
        z = 0;
    }
}

Point2D point1 = new Point2D(0, 1);
Point2D point2 = new Point2D(1, 3);
point.x;
```

`this`代表“当前实例”。`this.x`就是当前实例的成员变量x。

一般不用写this，像上面的z默认指的就是成员变量z，但是x和y和参数重名了，需要加this区分。

-----
# 成员方法
这里开始OOP

回到灵魂三问：

当我们看到一个变量的时候，它的类型会给我们一些额外的信息，比如：
- 它里面存储什么数据？
- 它在这里是被用来干什么的？
- ## 它能支持什么操作？

拿之前的类型举点例子：
- `int` 可以自增自减
- `String` 可以取某一个字符`charAt()`，可以取子串`substr()`
- `point` 目前啥也做不了。正常，一个点能做什么呢？
- `Grid` 目前也啥都做不了，这肯定不行，格子负责管理上面的棋子，总得做点啥吧..

-----
# 成员方法
这里开始OOP

成员方法用于实现类型应该支持的操作。

比如，我们的`Grid`类需要能够判断自己上面有没有棋子：
``` java
class Grid {
    int type; // bad，正确写法暂时超纲了
    Piece piece; // 可以为null
    public boolean hasPiece() {
        return piece == null;
    }
}
```
这里的`hasPiece`方法使用实例的成员变量piece，判断这个变量是否为null。逻辑很简单。

``` java
Grid grid = board.grids[0][0];
grid.hasPiece(); // ok
// Grid.hasPiece(); // err
```

需要一个实例来调用成员方法。上面的“实例”一页讲了为什么。（需要一个具体的格子才能知道里面有无棋子）

-----
# 等一下...为什么要成员方法？
OOP的理念其一

``` java
class Grid {
    int type; // bad，正确写法暂时超纲了
    Piece piece; // 可以为null
}

public static boolean hasPiece(Grid grid) {
    return grid.piece == null;
}

hasPiece(grid);
```

我们完全可以写一个非成员方法来做这件事，为什么要搞成员方法？（C语言是这么做的，当时还没有OOP）

因为...OOP的理念是把**数据**和**对数据的操作**都放在一起，扔进一个类里。这么做的优点：
- 方便找（对这个类型的所有操作都写在这个类里，详见刚才的构造方法例子）
- 访问控制
- 封装

-----
# 访问控制
不造屎山的尝试

``` java
public class Board {
    public Grid[][] grids;
}
```

Board类设计的时候，对于grid变量的使用方法只有一种：每回合把里面的棋子搬来搬去。
``` java
board.grids[i][j].piece = board.grids[m][n].piece;
board.grids[m][n].piece = null;
```

你的队友有一天酒后写代码，在Board类外面写出了惊世骇俗的：
``` java
board.grids = new Grid[10][10];
```

你合并了他的代码，观察到玩家走了几步棋之后，整个棋盘都被清空了！！！！！！

于是你debug十小时，发现了这一行。你的心态崩了。

程序员们喜欢偷懒，于是有了一劳永逸解决问题的方法：让`board.grids`在类外不可见，只能通过成员方法修改

-----
# 访问控制
不造屎山的尝试

``` java
public class Board {
    private Grid[][] grids;
    public void movePiece(int i, int j, int m, int n) {
        grids[i][j].piece = board.grids[m][n].piece;
        grids[m][n].piece = null;
    }
}

// board.grids[i][j] // err! private access.
board.movePiece(0, 0, 1, 1);
```

`private`关键字：让一个成员变量只能被成员方法访问。

然后，通过限制成员方法能做的事，就实现了...对“如何访问和修改数据”的限制。

就像`String`没有`setChar()`方法，不支持改字符串里的单个字符一样。

btw，能private变量自然也能private方法，意思是这个方法只能被成员方法访问。

-----
# Getter & Setter
还是访问控制

``` java
public class Player {
    private String name;
}
```

Getter和Setter本质上就是上一页的内容——你的成员变量在访问和修改时需要受限。

比如说，这个name只能在构造器里被设置，一旦被设置就不能被修改。那么就是只有Getter没有Setter

``` java
public class Player {
    private String name;
    public String getName() { return name; }
}
```

又比如说，name还是不能被修改，这次获取只能统一获取information而非单独的name，那么就是：
``` java
public class Player {
    private String name;
    private int id;
    public String getInformation() { return name + " " + id; }
}
```

-----
# Getter & Setter
还是访问控制

又比如说，name能单独获取能被修改，但修改时需要检查格式：
``` java
public class Player {
    private String name;
    public String getName() { return name; }
    public void setName(String name) { 
        if (checkFormat(name)) 
            this.name = name; 
    }
}
```

以及很常见的name能单独获取能被修改并且没有任何其他限制：
``` java
public class Player {
    private String name;
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
}
```

这里不把name直接搞成public的原因...习惯，以及如果以后突然要改getter和setter的行为，方便改。

-----
# final
好用，多用

这个name只能在构造器里被设置，一旦被设置就不能被修改。那么就是只有Getter没有Setter

``` java
public class Player {
    private String name;
    public String getName() { return name; }
}
```

但是有一个问题：成员方法还能修改name。
``` java
public class Player {
    private final String name;
    // public void method() { name = ""; } // err!
}
```

把name设为final，就完全满足“必须在构造器里被设置，一旦被设置就不能被修改”。

好用。

-----
# 封装
和刚才的差不多

这个本质上还是上面的内容——只能通过成员方法去修改变量。也可以说封装是基于访问控制的。

正式一点的说法：
- 将使用者所不需要知道的细节隐藏，只暴露需要了解的内容和功能。
- 类的内部数据及操作细节在类内完成，不允许外部干涉，仅对外暴露必要的方法用于使用

人话：写尽可能少的public成员方法，比如我要实现一个生成随机数的类：

``` java
public class Random {
    private int a, b, c;
    public String nextInt() { /* complex math from a, b, c */ }
}
```

这个类应该只生成随机数，有一个`nextInt()`就足够完成功能了，**不应该**再写`getA()`, `getB()`这些成员方法。

Why？

-----
# 封装
和刚才的差不多

假设你写了`getA()`等等和生成随机数相关的方法...

这个类写出来一段时间之后，你发现现有的数学方法有缺陷，要换一个方法生成随机数

``` java
public class Random {
    private int d, e, f, g, h;
    public String nextInt() { /* complex math from d, e, f, g, h */ }
    public int getA() { /* ????? */}
}
```

getA没法写了。因为新方法没有a这一项。

而且大部分情况下getA删不掉，因为其他的代码很可能因为奇怪的需求调了getA()。删了就得把他们改掉。寄。

所以，**将使用者所不需要知道的细节隐藏，只暴露需要了解的内容和功能。**

-----
# 封装
和刚才的差不多

换一个现实点的例子：

``` java
public class Board {
    private Piece[][] grids;
    public void movePiece(int i, int j, int m, int n);
    public void getPiece(int i, int j);
    public Piece[][] getGrids(); // NOOOOOOOOO
}
```

project写了一半，发现Piece二维数组不行，得再加一层Grids。

``` java
public class Board {
    private Grid[][] grids;
    public Piece[][] getGrids(); // 寄。
}
```

这时候代码里可能充斥着各种getGrids()。只能花几个小时都改一遍。

合理的做法可能是把所有需要getGrids()的需求都做到Board类里面。

-----
# 封装
和刚才的差不多

封装的另一个好处是方便分工，节约你和你队友的脑细胞。

比如Board类是你写，你的队友负责写UI：
- 队友要显示棋盘，问你要一个`getGrid(i, j)`方法。
- 队友要清空棋盘，问你要一个`clearBoard()`方法。
- 队友要动棋子，问你要一个`movePiece()`方法。

队友写的时候只需要调一下就好了，不需要知道你是怎么管理棋盘的。

同样，你不需要知道你的这些方法是如何被调用的，你只要处理好你的功能就行了。

不止是棋盘，包括玩家系统，AI，联网对战，所有的一切都是这样。

一个复杂系统的几个部分通过一些非常少的方法相互连接，这样任何一部分的变动都不会影响到其他部分，写这几部分的不同的人只需要知道其他部分的大概而非细节。高效。

-----
# 不要过度设计
<div />

提了这么多，了解即可。

刚才说的都是理想情况，你们才刚接触代码，写的时候知道大概有这个事就行了。

实际上大厂的代码也全都是屎。。。

只有先造出来屎山，才能知道什么写法容易造屎山，才能知道怎么设计可以避免。

project大胆的写，能跑比什么都重要！

（以及，不要怕代码写错。

（2014年图灵奖得主Michael Stonebraker在一次访谈中提到, 他当时花了5年时间开发了世界上第一个关系数据库系统Ingres, 其中90%的时间用于将它运行起来. 也就是说, 在开发过程中, 有90%的时间系统都是运行不起来的, 是有bug的, 需要调试.

-----
# 非成员方法
<div />

讲OOP之前的方法都是这么写的：
``` java
public static void f() {

}
```

`static` 关键字代表“不访问成员变量，和实例没有关系”。

比如 `Math.abs`。它只是接受一个数输出一个绝对值，和什么类的什么实例一点关系都没有。

调用这种方法只需要`Class.methodName()`就好，类名+方法名。类名只是用来定位的。

非成员方法有点像int的加号`a + b`，输入两个int，不改变它们本身，返回一个新int。

成员方法有点像int的自增`a++`，改变一个实例本身。

具体什么时候用非成员方法我也讲不清楚，凭感觉吧。。

-----
# 非成员变量和方法
<div />

另外，还有全局变量`public static int a;`这个代表“该变量全局可见且只有一份”。

刚才提到`public int a;`只是声明类的每一个实例里面应该有一份a，没实际创建，

加了static就实际创建了，和方法里面写`int a`一样。就当一个方法里的变量用。

``` java
public class A {
    int a = 0;
    static int b = 0;
}

psvm() {
    souf("%d %d\n", A.a, A.b); // A.a报错
    A x = new A(), y = new A();
    x.a = 1; x.b = 1;
    souf("%d %d\n", y.a, y.b); // 0, 1
}
```

访问这个变量：`A.a`，类名同样只是用来定位的。例子里用`x.a`访问是不好的！

如果你有一个程序到处都用到的工具方法，或者到处都用到的变量，static！

-----
# 注意事项
希望你们没忘上节课的内容...

``` java
public class A {
    int[] ref;
    public A(int[] a) { ref = a; }
}

int[] arr = new int[]{1, 2, 3};
A a1 = new A(arr);
A a2 = new A(arr);

a1.ref[0] = 10;
// a2.ref; {10, 2, 3}
```

成员变量和普通变量还有函数传参什么的一样，该引用类型还得是引用类型！！！！！！
