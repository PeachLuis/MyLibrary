---
layout:     post
title:      java核心
subtitle:   java核心技术卷Ⅰ
date:       2020-1-24
author:     PeachLuis
header-img: 
catalog: true
tags:
- java

---

[TOC]



# 第三章 Java的基本程序设计结构

## 3.1 一个简单的java应用程序

```java
public Class Test
{
	public static void main(String[] args)
    {
		System.out.print("Hello,world!");
    }
}
```

要在命令行中运行此代码，需要输入以下命令：

```dos
javac Test.java
java Test
```

​		javac命令是编译java文件为可执行的 .class 文件

​		java命令是运行 .class 文件，所以不用加后缀

**注意：**

  1. Java中所有的函数都属于某个类的方法，因此，Java中的main方法必须有一个外壳类；

  2. 该java文件的命名必须和代码中public class的命名一致，比如这里必须是Test，否则无法编译通过；

  3. 如果采用命令行的方式编译并运行java代码，需要注意字符串中的编码格式，可以以情况设置；

  4. 因为java区分大小写，所以注意代码大小写；

  5. java类名一般采用首字母大写，多个单词采用驼峰命名法；

  6. 所有语言都中英文敏感，所以逗号，分号，括号等注意中英文；

  7. 如果直接用idea打开这个java代码，无法运行，左下角有黄色 J 警告标志，这是因为没有设置output的路径，无法编译；解决方法如下：

     1. 先移除右边的出问题的java代码，然后再将有问题的代码加上**Sources**

        

     2. 在Modules设置path里勾选”Inherit project compilepath” ，如果已经勾选则不管它

        

     3. 设置Project中的”Project compiler output”，路径设置为你的项目下的out文件夹，这个文件夹是用来存储编译后的 .class 文件

        

## 3.2 注释

总共三种注释方法，如下：

1. //用于简单的注释，一般用于单行或者比较少行的注释；
2. /* 这是一段注释 */ 一般用于较长的注释；
3. /** 这是一段注释 */ 可以用来生成javadoc文档，可用在下面几个方面：
   1. 包
   2. 公有类与接口
   3. 公有的和受保护的构造器和方法
   4. 公有的和受保护的域

```java
/**
 * 学生类 Student
 */
public class Student
{
    //姓名
	private String name;
    
    //年龄
    private int age;
    
    //性别
    private String sex;
    
    /**
     * 构造函数，用给定参数构造一个Student对象
     * @param name	姓名
     * @param age	年龄
     * @param sex	性别
     */
    public Student(String name, int age, String sex)
    {
		this.name = name;
        this.age = age;
        this.sex = sex;
    }
    
	/**
     * 得到Student对象的name
     * @return	Student对象的name
     */
    private String getName()
    {
		return name;
    }
}


```

**具体内容可参看《java核心计数卷Ⅰ》p140**

## 3.3 数据类型

**分类：八大基本数据类型+引用类型：**

1. 4整型：
   1. byte（8位1字节）范围 -2^7——2^7-1，默认0
   2. short（16位2字节）范围 -2^15——2^15-1，默认0
   3. int（32位4字节）范围 -2^31——2^31-1，默认0 （最常用）
   4. long（64位8字节）范围 -2^63——2^63-1，默认0l
2. 2浮点：
   1. float（32位4字节）默认：0.0f 必须标注f
   2. double（64位8字节）默认：0.0d或省略0.0 （最常用）

3. 1字符：
   1. char（8位1字节）默认' '，为空
4. 1布尔
   1. boolean（只有true和false两种取值）默认为false

5. 引用
   1. 比如像String类一样的其他用户自定义类或者java自带类，**默认为null**

**注意：**

1. 基本数据据类型有对应的BigNumber，其为Java对象，可以表示任意精度；

2. java中整型值不能和布尔值进行相互转换，c++中可以互相转换；

3. 从Java7开始，加上前缀0b或者0B可以表示二进制数；还可以在字面量比如整型数隔三加下划线"__"，java编译器会除去这些下划线；

4. 浮点数值计算可能存在出乎意料的误差，这是由于浮点数是由二进制系统表示的，所以在金融计算中不能使用浮点类型，而应该使用BigDecimal类；

5. 浮点型表示溢出和出错情况的三种特殊的浮点数值：

   - 正无穷大
   - 负无穷大
   - NaN（不是一个数字）

   对应的Java中的表示为：

   - Double.POSITIVE_INFINITY
   - Double.NEGATIVE_INFINITY
   - Double.NaN

   检测一个特定值为非数值采取下面这种方法：

   ```java
   	if（Double.isNaN(x)）	//检查x是否是非数值
   ```

## 3.4 变量

1. java不像c++一样区分变量的声明和定义，但是java中变量必须被初始化赋值；

2. 变量名一般采取小写驼峰命名法，常量一般采用全大写；

3. 使用final关键字指示常量，static final 指示类常量，定义在main方法之外，常用的定义和使用方式如下：

   ```java
   	public static final PI = 3.1415926;		
   	int a = Math.PI;		//因为PI常量定义在Math类中
   ```

## 3.5 运算符

1. +，-，*，/，%

2. Math函数当中的数学方法，常用方法如下：

   - Math.sqrt(x);		//对x开根号 
   - Math.pow(x,a);    //求x的a次方
   - Math.abs(x);        //对x求绝对值
   - Math.ceil(x);        //x向上取整
   - Math.floor(x);     //x向下取整
   - Math.round(x);    //对x四舍五入，返回结果类型为long
   - Math.PI;                //∏=3.14159265358979323846
   - Math.E;                  //e=2.7182818284590452354

3. floorMod，floorDiv

4. **注意**，在Math类中，为了达到最快的性能，所有的方法都使用计算机中的浮点运算，所以会导致有时计算不准确；**但是如果结果的准确性更加重要的话**，那么就应该使用StrictMath类，才能得到准确的运算结果；

5. 类型转换：

   1. 小范围 -> 大范围：结果准确，自动转换类型
   2. 大范围 -> 小范围：结果可能不准确，需要强制类型转换

   （将浮点型转换为整型的话，是通过截断小数部分，所以会造成信息丢失）

6. 自增与自减：

   ```java
   int a =b =2 ;
   int temp1 = ++a;	//此时a=3，temp=3；因为前自增先进行a的其他运算，再进行自增运算；（前缀自增）
   int temp2 = b++;	//此时b=3，temp=2；因为后自增先进行自增，再进行其它运算；（后缀自增）
   ```

7. 位运算符与关系运算符

   位运算符：&，|，^，~

   条件运算符：&&，||，!，>，<，>=，<=，=

8. 括号和运算符级别：

   《Java核心技术卷Ⅰ》p44表3-4

9. 枚举类型：

   如果没有对枚举赋值，则按顺序依次被初始化为0，1，2,....,n

   例子：

   ```java
   	enum Size{SAMLL,MEDIUM,LAGGE,EXTRA_LARGE};
   	Size s = Size.LARGE;
   ```

10. instanceof ：

    判断一个对象是否是一个类的实例

## 3.6 字符串

### 3.7.1 基础知识

1. String和StingBuilder的API文档

2. Java没有内置的字符串类型，而是在标准Java类库中提供了一个预定义类——String，每个用双引号括起来的字符串都是String类的一个实例。

3. String类的subString（）方法可以得到一个字符串的子串，得到的子串即使内容和另外一个字符串内容相同，但是`==`得到的使false，不过使用equals方法得到的还是true，所以字符串的比较不能用`==`，必须用equals方法。

   ```java
   String s = "abcd";
   //普通方法，接受两个整型，得到s中下标从0到2的子串，包括0不包括2
   String temp1 = s.subString(0,2);	//temp1 = "ab"
   //接受一个整型，得到从该下标到结尾的子串
   String temp2 = s.subString(1);		//temp2 = "bcd"
   ```

4. 拼接，String对象可以互相拼接，使用`+`连接，拼接得到的字符串又是一个新的String实例；字符串和非字符串的值进行拼接，会将后者转换成字符串（即，任何一个java对象都可以转换成字符串）；
   如果需要把多个字符串放在一起，并用一个分隔符分开，可以使用join方法

   ```java
   String lal = String.join("/", "S", "M", "L");
   System.out.println(lal);
   //得到；S/M/L
   ```

5. String为不可变字符串，如果修改字符串就只能使用拼接，这样会造成性能低下，不过String被设计为如此不让修改，如果想要高效地实现修改字符串，可以使用StringBuilder，不可变字符串的优点：编译器可以让字符串共享；

6. java中的字符串不像c++中一样等于一个char数组，而是类似于char*指针，**因为java有自动垃圾回收机制**，所以尽管是将字符串放置在堆内存中，但是程序员可以不用管它，虚拟机会自动回收垃圾。

7. **检测字符串是否相等**：

   1. 想要监测两个字符串是否相等，而不区分大小写，可以使用equalsIgnoreCase方法；
   2. **必须使用`equals`方法，不能使用`==`；**

   ​	如果比较的是两个值相同的String类型字符串用“==“结果为true，这是因为String字符串是共享的；如果换	成”+“或者substring，得到的字符串不是共享的，即使值相同，结果也会是false；

   ​	（上面的内容是《java核心技术卷一》当中的内容，但是在IDEA中发现使用“+”连接的字符串仍旧相等，只有使用substring结果是false，结果如下：）

   ```java
    public static void main(String[] args) {
           System.out.println("\"abc\".equals(\"abc\")" + "：" + "abc".equals("abc"));
           System.out.println("\"abc\".equals(\"a\" + \"b\" + \"c\")" + "：" + "abc".equals("a" + "b" + "c"));
           System.out.println("\"abc\".equals(\"abcd\".substring(0,3))" + "：" + "abc".equals("abcd".substring(0, 3)));
           System.out.println("\"abc\" == \"abc\"" + "：" + ("abc" == "abc"));
           System.out.println("\"abc\"==(\"a\"+\"b\"+\"c\")" + "：" + ("abc" == ("a" + "b" + "c")));
           System.out.println("\"abc\" == \"abcd\".substring(0, 3)" + "：" + ("abc" == "abcd".substring(0, 3)));
       }
   ```

   ```
   //结果
   "abc".equals("abc")：true
   "abc".equals("a" + "b" + "c")：true
   "abc".equals("abcd".substring(0,3))：true
   "abc" == "abc"：true
   "abc"==("a"+"b"+"c")：true
   "abc" == "abcd".substring(0, 3)：false
   ```

8. 空串与Null串

   1. 空串判断：

      ```java
      	if(str.length() == 0)	//法一
          if(str.equals(""))		//法二
      ```

   2. Null串判断

      ```java
      	if(str == null)
      ```

   3. **两种都需要判断（&&采用短路判断方式，必须要首先检查str不为Null，因为在null值上调用方法会出现错误）**

      ```java
      	if(str != null && str.length() != 0)	
      ```

9. 码点与代码单元：大多数常用的Unicode字符使用一个代码单元就可以表示，而辅助字符需要一对代码单元表示，如使用 UTF-16 编码表示字符⑪(U+1D546) 需要两个代码单元。调用 char ch = sentence.charAt(l) 返回的不是一个空格，而是⑪的第二个代码单元。为了避免这个问题， 不要使用 char 类型。 这太底层了。；

   **注意，length方法得到的是代码单元的数量。**

   码点即是字符的Unicode编码，如a的码点为为97；码点可以通过s.codePointAt方法得到，也可以通过字符强转为int类型得到，但是一种更方便得到码点的方法是使用codePoints方法

   ```java
   String s = "abcd";
   int[] co = s.codePoints().toArray();
   System.out.println(Arrays.toString(co));
   //输出：[97, 98, 99, 100]
   ```

   反之，将一个码点数组转换为一个字符串，可以使用构造函数

   ```java
   String str = new String(codePoints,0,codePoints.length);
   ```

   

10. **使用可变长度的字符串的两个类**：（对象的值能进行多次修改，而且不产生新的未使用对象） 

   1. StringBuilder：用于**单线程**中编辑，**线程不安全（不能同步访问），但是速度快**
   2. StringBuffer：用于**多线程**，**线程安全，但速度较之较慢**

   （两种类的API是相同的，但是单线程中StringBuilder的效率更高）

   面试题：

   **JAVA 中的 StringBuilder 和 StringBuffer 适用的场景是什么？**

   最简单的回答是，stringbuffer 基本没有适用场景，你应该在所有的情况下选择使用 stringbuiler，除非你真的遇到了一个需要线程安全的场景，如果遇到了，请务必在这里留言通知我。

   然后，补充一点，关于线程安全，即使你真的遇到了这样的场景，很不幸的是，恐怕你仍然有 99.99....99% 的情况下没有必要选择 stringbuffer，因为 stringbuffer 的线程安全，仅仅是保证 jvm 不抛出异常顺利的往下执行而已，它可不保证逻辑正确和调用顺序正确。大多数时候，我们需要的不仅仅是线程安全，而是锁。

   最后，为什么会有 stringbuffer 的存在，如果真的没有价值，为什么 jdk 会提供这个类？答案太简单了，因为最早是没有 stringbuilder 的，sun 的人不知处于何种愚蠢的考虑，决定让 stringbuffer 是线程安全的，然后大约 10 年之后，人们终于意识到这是一个多么愚蠢的决定，意识到在这 10 年之中这个愚蠢的决定为 java 运行速度慢这样的流言贡献了多大的力量，于是，在 jdk1.5 的时候，终于决定提供一个非线程安全的 stringbuffer 实现，并命名为 stringbuilder。顺便，javac 好像大概也是从这个版本开始，把所有用加号连接的 String 运算都隐式的改写成 stringbuilder，也就是说，从 jdk1.5 开始，用加号拼接字符串已经没有任何性能损失了。

   如诸多评论所指出的，我上面说，"用加号拼接字符串已经没有任何性能损失了"并不严谨，严格的说，如果没有循环的情况下，单行用加号拼接字符串是没有性能损失的，java 编译器会隐式的替换成 stringbuilder，但在有循环的情况下，编译器没法做到足够智能的替换，仍然会有不必要的性能损耗，因此，用循环拼接字符串的时候，还是老老实实的用 stringbuilder 吧。

### 3.7.2 格式化

1. 格式化输出可以使用System.out.printf，而如果用字符串存储，可以使用format方法。

   ```java
   String name = "luis";
   int age = 20;
   String s = String.format("Hello, %s. Next year, you'll be %d",name,age);
   ```

2. printf转换符
   ![image-20201125154746341](img/image-20201125154746341.png)

3. 用于printf的标志
   ![image-20201125154817539](img/image-20201125154817539.png)

4. 基于完整性的考虑， 下面简略地介绍 printf方法中日期与时间的格式化选项。在新代码中， 应当使用卷 II 第 6 章中介绍的 java.time 包的方法。 不过你可能会在遗留代码中看到 Date 类和相关的格式化选项。格式包括两个字母， 以 t 开始， 以表中的任意字母结束。

   ![image-20201125160139632](img/image-20201125160139632.png)

   如下代码：

   ```java
   System.out.printf("%tc",new Date());
   //输出：周三 11月 25 15:51:38 CST 2020
   ```

5. 从表 3-7 可以看到， 某些格式只给出了指定 丨期的部分信息 t 。例如， 只有 FI 期 或 月 份 如 果需要多次对口期操作才能实现对每一部分进行格式化的 Q 的就太笨拙了为此， 可以采用一 个格式化的字符串指出要被格式化的参数索引。索引必须紧跟在 ％ 后面， 并以 $ 终止。 例如（提示，参数索引是从1开始），

   ```java
   System.out.printf("l$s %2$tB %2$te, %2$tY", "Due date:", new DateQ);
   //打印：Due date: February 9, 2015 
   ```

    还可以选择使用 < 标志它指示前而格式说明中的参数将被再次使用。也就是说， 下列 语句将产生与前面语句同样的输出结果： 

   ```java
   System.out .printf("%s %tB %<te, %<tY", "Due date:", new DateO);
   ```

   

## 3.7 输入输出

1. 在控制台的简单输入输出:

   1. 控制台输出：

      ```java
      	System.out.print("输出信息1");	//输出内容后，接下来的内容紧跟该输出内容后
      	System.out.println("输出信息2");	//输出内容后换行，常用作无参，表示空一行
      	System.out.prnitf("%8.2f",x);	//格式化输出，更多内容参看《Java核心技术卷Ⅰ》p58，或者菜鸟教程JAVA
      	（每一个以%字符开始的格式说明符都用相应的参数替换）
      ```

   2. 控制台输入（调用Scanner类进行输入）：

      ```java
      	Scanner in = new Scanner(System.in);
      	in.nextInt; 		//in.nextDouble,in.nextLine,常用方法参看p57页Scanner类
      ```

      需要知道的是：in.next读取的是一段不包括空格的字符串，而不是一个字符；

      ```java
      	public static void main(String[] args) {
              Scanner in = new Scanner(System.in);
              ArrayList<String> arrayList = new ArrayList<>();
              while (!in.hasNext("exit")) {
                  arrayList.add(in.next());
              }
              for (String a : arrayList) {
                  System.out.println(a);
              }
          }
      //输入：（注意exit也必须是一个next，所以不能包含其他字符）
      我是1号 我是2号 我是3号，这是附加内容 exit
      //输出：
      我是1号
      我是2号
      我是3号，这是附加内容
      ```

      使用Console类进行控制台输入不可见的输入：

      > 注意：
      >
      > 1. 这个在输入密码的时候不是全是***，而是完全不可见的状态
      > 2. Console这个类在IDEA等编译器下不能使用，会报空指针错误；在我尝试下，目前我知道的是在Windows的Power Shell下可以编译并运行
      >
      > 

      ```java
          public static void main(String[] args) {
              Console console = System.console();
              String name = console.readLine("Name:");
              char[] password = console.readPassword("Password:");
              System.out.println();
              
              System.out.println("Name:"+name);
              System.out.print("Password:");
              for (char aPassword : password) {
                  System.out.print(aPassword);
              }
          }
      ```

2. 文件输入和输出（更常用的方法应该是IO流中的工具类）

   1. 如果对文件进行读取，需要一个用File对象构造一个Scanner对象，如下所示

   ```java
   	Scanner in = new Scanner(Paths.get("myfile.txt"),"UTF-8");
   	//如果文件名中包含反斜杠，记住每个反斜杠之前再加一个额外的反斜杠
   
   	//注意：如果使用下面这种方式，则Scanner将字符串解释为数据，而不是文件名	
   	Scanner in = new Scanner("myfile.txt"); 
   ```

   > 注释： 在这里指定了 UTF-8 字符编码， 这对于互联网上的文件很常见（不过并不是普遍 适用）。读取一个文本文件时，要知道它的字符编码—更多信息参见卷 n 第 2 章。如果 省略字符编码， 则会使用运行这个 Java 程序的机器的“ 默认编码”。 这不是一个好主意， 如果在不同的机器上运行这个程序， 可能会有不同的表现。

   2. 如果写入文件，需要构造一个PrintWriter对象，如下所示

   ```java
   	PrintWriter out = new PrintWriter("myfile.txt"，"UTF-8");
   	//如果文件不存在，则创建该文件，和Scanner不同
   ```

   注意：

    1. 在IDE中启动路径由IDE控制，得到路径位置用下面语句

       ```java
       	String dir = System.getProperty("user.dir");
       ```

   2. 定位文件可以使用相对路径和绝对路径，如果觉得定位文件位置比较烦恼，可以使用绝对路径,例如：“ c:\\mydirectory\\ myfile.txt ” 或者“ /home/me/mydirectory/myfile.txt” 、

   3. 如果文件名中包含反斜杠符号，就要记住在每个反斜杠之前再加一个**额外的反斜杠**

   4. **如果用一个不存在的文件构造一个Scanner进行文件读取，或者用一个不能被创建的文件名构造一个PrintWriter，那么就会发生异常，而且Java编译器认为这些异常比”被0除“更严重，所以我们需要在方法中用throw子句标记，如下所示：**

      ```java
      public static void main(String[] args) throw IOException
      {
          //假如”myfile.txt“不存在
      	Scanner in = new Scanner(Pahts.get("myfile.txt"),"UTF-8");
          ...
      }
      ```

    5. 可以构造一个只带有一个字符串参数的Scannner，但是这个Scannner将字符串解释为数据，而不是文件名。

    6. 当使用命令行方式启动一个程序时，可以利用Shell的重定向语法将任意文件关联到System.in和System.out，这样就不必担心处理IOException异常了

       ```powershell
       	java MyProgram <myfile.txt> output.txt
       ```

## 3.8 控制流程

### 3.8.1 块作用域

1. 由一对大括号括起来的若干条简单语句叫块；
2. 块可以嵌套，但是不能在嵌套的两个块中声明同名的变量；（注：在c++中在嵌套的语句中重定义一个变量，内层定义的变量会覆盖外层定义的变量，java认为这样可能会导致程序设计错误，因此不允许这样做）
3. 块中定义的变量一般生命周期只覆盖该块及其嵌套的子块，除非是常量和类变量；

### 3.8.2 条件语句

1. if-else

### 3.8.3 循环

1. while语句

2. do-while语句

3. break；分为带标签和不带标签的写法，不带标签的会跳出当前循环，而带标签的会跳过标签下的循环语句；break只能跳出循环，不能跳进循环；

   ```java
   int counter = 0;
   read_data:
   while(true){
       counter++;
   	for(int i=0;i<10;i++){
           counter++;
           if(counter == 4){
               break read_data;
               //跳出while循环
           }
       }
   }
   ```

4. continue；直接开始下一次循环；

### 3.8.4 确定循环

1. for循环

2. 注意：for循环使用浮点数的时候需要注意，由于0.1无法精确地用二进制表示，所以可能由于舍入的误差，最终得到到精确值，程序可能永远不会结束。

3. 计算从n个数字中随机抽取k个数字，使用如下方法:

   ```java
   int lotteryOdds = 1;
   for(int i=0; i<k; i++)
   {
   	lotteryOdds = lotteryOdds * (n-i+1)/i;
   }
   ```

4. for each循环：依次处理数组中的每个元素，而不必为指定下标值而分心（其实是迭代器Iterator的实现）

   ```java
   	for(变量 : 集合/实现了Iterable接口的类对象)
       {
   		statement;
           ...
       }
   	//例如：
   	for(int element ; a)
       {
   		System.out.println(element);
       }
   	
   ```

   我们知道了可以用循环语句来进行打印数组内容，这里还有一种更方便的方式，采用java.util包内的Arrays类中的toString方法，即可返回一个包含数组元素的字符串，这些元素被放置在括号内，用逗号分隔（针对基本数据类型数组或者多重数组，有一个toString（Object[]）的方法，但是只会输出引用对象的地址

   ```java
   	int[] a= {2,5,8,6};
   	System.out.println(Arrays.toString(a));
   	//输出：[2,5,8,6]
   ```

### 3.8.5 多重选择：switch语句

```java
	//例子
	Scanner in = new Scanner(System.in);
	System.out.println("请输入一个选项：1，2，3/n");
	int choice = in.nextInt();
	switch(choice)
    {
        case 1:
            ...;
            break;
        case 2:
            ...;
            break;
        case 3:
            ...;
            break;
        default:
            ...;
            break;
    }
```

注意：

1. 特别注意break的书写，如果不添加break，程序就会一直执行后面的case语句，例如，都不写break，如果输入1，则程序会执行case1，2，3，default的语句内容。
2. case标签可以是**char、byte、short或int的常量表达式或者其包装类，枚举常量**，从java SE 7 开始，还可以是字符串字面量，其他的数据类型不支持，**所以浮点类型也是不支持的**；

## 3.9 大数值

1. 由于基本的整型和浮点型数据类型不能够满足需求，所以可以使用java.math包中的两个类：

   **BigInteger，BigDecimal**

   可以处理任意长度数字序列的数值

2. 可以使用静态的valueOf方法将普通的数值转换为大数值：

   ```java
   	BigInteger a = BigInteger.valueOf(100);
   ```

3. 不能使用人们熟悉的（+，-，*，/，%）操作符，因为是自定义的类，需要使用对应的方法进行运算；

4. 常用的BigInteger构造方法：

   ```java
   BigInteger a = new BigInteger("111");
   BigInteger b = BigInteger.valueOf(1);
   ```

   

## 3.10 数组

数组中常用使用方式如下：

```java
	int[] a;	//声明一个整型数组a
	int[] a = new int[10];	//实例化一个长度为10的整型数组
	int[] b = {1，5，6，4，4};	//或采取这种方式实例化，不需要用到new
	int[] b = new int[]{1，5，6，4，4};	//也可以加上new操作
	System.out.println(a.length);	//得到数组的长度
```

1. 创建一个数组后，如果没有初始化数组中的值，会自动初始化为默认值：整型为0，浮点型为0.0，char为空，布尔型为false，String及其他引用类型为null；
2. 一旦创建了数组，就不能再改变它的长度，如果需要再运行过程中改变数组长度，需要用到另外一种数据结构----数组列表（ArrayList）；
3. 数组的输出可以使用循环遍历，也可以使用java.util包中的Arrays类中的toString方法：Arrays.toString()；
4. 匿名数组
5. 在java中，允许数组长度为0。注意，数组长度为0

### 3.10.1 数组和字符串的相互转换

1. 基本类型转字符串：

   1. 使用Arrays.toString()方法，得到的字符串为中括号括起，中间逗号隔开，**此方法对所有基本数据类型甚至是String或者其他自定义引用类型都适用**，的如：[1,2,3]
   2. 如果是多维数组，则使用Arrays.deepToString()方法
   3. 使用StringBuffer或者StringBuilder类，利用循环来构造字符串，不推荐使用String来构造，因为String为不可变字符，将数组中元素依次进String中，每一次都会产生新的字符串，空间消耗大（虽然我们知道一条比较短的使用“+”连接的String字符串在jdk1.5之后会被默认的转化为StringBuilder，几乎没有什么损耗，但是在循环的时候，如果你仍旧使用“+”来修改String，那对内存的损耗是巨大的）

2. 字符串转基本类型数组（只能转换为char数组）：

   1. 使用String类的toCharArray（）方法，一行代码解决

   2. 使用StringBuffer或者StringBuilder类，通过循环赋值到新建char数值中。

   3. 每一个基本类型的包装类都有一个将指定字符串转为该包装类的方法，如

      ```java
      String a = "123";
      int num = Integer.parseInt(a);
      System.out.println(num + 3);	
      //输出：
      126
      ```

> 具体代码内容，参看Chapter3 / 3.10 / ExchangeTest

### 3.10.2 数组拷贝

1. 在java中，允许将一个数组变量拷贝给另一个数组变量，这时，两个变量将引用同一个数组（浅拷贝）

2. 如果要将一个数组的所有值拷贝到一个新的数组中去，可以使用Arrays类的copyOf方法（深拷贝）

   **注意：c++中深拷贝是使用new操作符即可，java中使用new操作符再进行拷贝，会提示多余；java中实现深拷贝使用Arrays.copyOf()方法（适用于所有基本类型和引用类型数组），或者你直接自己实现，创建新的同类型数组变量然后循环赋值**

3. 注意：java中的数组相当于c++中的指针，但是不能使用a++得到下一个元素（c++可以通过直接地址++得到下一位）

### 3.10.3 java.util.Arrays

这里简单提一下Arrays常用的API

```java
	1. static String toString(T[] a){}
		//返回a数组中所有数据元素的字符串，这些数据元素被放在[]中，用逗号分隔
		//对所有基本数据类型和和String适用，对其他自定义类不适用，只会输出其地址
	2. static T copyOf(T[] a, int length){}
	3. static T copyOf(T[] a, int start, int end){}
		//start	起始下标（包含这个值）
		//end	终止下标（不包含这个值）
		//适用于所有数据类型和引用类型的数组
	4. static void sort(T[] a){}
		//采用优化的快速排序对数组进行排序
	5. static int binarySearch(T[] a, T tag){}
	6. static int binarySearch(T[] a, int start, int end, T tag){}
		//采用二分查找查找tag，查找成功给出下标，否则返回一个负数
	7. static void fill(T[] a, T v){}
		//将数组的所有元素设置为v
	8. static boolean equals(T[] a, T[] b)
```

### 3.10.4 多维数组

1. 一般常用是二维数组（矩阵），其他多维几乎用不到

2. 二维数组声明方法

   ```java
   	int[][] a = new int[2][3];
   	
   	int[][] b = {
           {1,2,3,4},
           {2,5,9,6}
       };
   ```

3. 二维数组的处理（使用双重for循环）

   ```java
   		for (int i = 0; i < a.length; i++) {
               for (int j = 0; j < a[i].length; j++) {
                   ...
                   ...
               }
           }
   ```

4. for each循环语句不能自动处理二维数组的每一个元素，要访问二维数组所有元素，如下：

   ```java
   		for (int[] row : a) {
               for (int[] value : row) {
                   do someting with value
               }
           }	
   ```

5. 要想快速打印一个二维数组列表，可以调用**Arrays.deepToString（）**

### 3.10.5 不规则数组

1. 注意：虽然看起来java中的数组和其他语言差别不大，但是java实际上没有多维数组，只有一维数组，多维数组被解释为“**数组的数组**”。

   > 代码参看 Chapter / 3.10 / LotterArray





# 第四章 对象与类

## 4.1 面向对象程序设计概述

1. 明白OOP的概念，在OOP中，数据放在第一位，然后再考虑操作数据的算法；

### 4.1.1 类

1. 由类构造对象（construct）的过程称为 **创建类的实例**
2. 封装是与对象有关的一个重要概念。实现封装的关键在于绝对不能让类中的方法直接地访问其他类的实例域，即在类的设计中，一般情况下**实例域用private修饰，方法用public修饰**
3. 通过扩展一个类来建立另外一个类的过程称为继承（inheritance），所有类都继承自一个超类——Object；

### 4.1.2 对象

通过下面对象的三个特性来运用OOP：

- 对象的行为（behavior）——可以对对象施加哪些操作，或可以对对象施加哪些方法？
- 对象的装填（state）——当施加那些方法时，对象如何响应？
- 对象标识（identity）——如何辨别具有相同行为与状态的不同对象？

### 4.1.3 识别类

面向过程的语言从main函数开始编写程序

面向对象的语言从设计类开始，然后再往每个类中添加方法，最后在main函数中调用

### 4.1.4 类之间的关系

类之间常见的关系有：

- 依赖（“uses-a”）：如果一个类的方法操作另一个类的对象（应该尽可能将相互依赖的类减至最少，即降低耦合）
- 聚合（“has-a”）：一个类A中包含另一个类B的对象
- 继承（“is-a”）：类B继承类A，则类B包含A的所有方法，还包含B中自己扩展的方法

1. 通常用UML绘制类图（可参看P94，或者自行搜索）

## 4.2 使用预定义类

### 4.2.1 对象与对象变量

要想使用对象，就必须首先构造对象，并指定其初始状态。然后，让对象使用方法。

java使用构造器来构造新实例，用来构造并初始化对象，如：

```java
	System.out.println(new Date());
	Date date = new Date();
```

1. 注意：一个对象变量并没有实际包含一个对象，而仅仅引用一个对象，必须初始化对象变量（即new操作，会给变量分配地址，也可以赋值为null，表示没有地址指向，）

   ```java
   	Date d;	//对象变量
   	Date a = new Date();
   
   	//初始化方法
   	d = new Date();		//动态构造，使用new
   	d = Date.；			//静态构造，工厂方法
       d = a;				//引用已初始化对象
   	d = null;			//表示这个对象变量目前没有引用任何变量
   ```

2. 在java中，任何对象变量的值都是对存储在另外一个地方的一个对象的引用，new操作符的返回值也是一个引用

3. 如果将一个方法应用于一个值为null的对象上，那么就会产生运行时错误，**注意：赋值为null之后绝对不能再调用方法，因为是在null上调用方法，所以会报空指针java.lang.NullPointerException**

4. java对象变量与c++中的对象指针类似，所有的java对象都存储在堆中，在c++中，指针运算容易造成内存溢出，但是在java中不存在，在java中如果使用一个没有初始化的指针，运行系统将会产生一个运行时错误，而不是生成一个随机的结果，同时不必担心内存管理问题，垃圾回收机制将处理相关事宜。

### 4.2.2 Java类库中的LocalDate类

1. Date类介绍（java.util.Date）：时间是距离UTC时间（与格林威治标准时间一样1970年1月1日00：00：00）的毫秒数（可正可负），这个点称为纪元（epoch）。**缺点：Date提供的日期处理并没有太大的用途，而且无法表示阴历和其他比如火星历**

2. LocalDate类介绍（java.time.LocalDate）：用来表示日历表示法，**构造LocalDate类必须使用静态工厂方法，不能使用构造器来构造其对象**，有以下两种构造方式：

   ```java
   	LocalDate now = LocalDate.now();
   	LocalDate now1 = LocalDate.of(2020, 12, 21);
   ```

3. 常用API:

   ```java
   	static LocalDate now();
   	static LocalDate of(int year, int month, int day);
   	LocalDate plusDays(int n);
   	LocalDate minusDays(int n);
   	int getYear();
   	int getMonthValue();
    	int getDayOfMonth();
   	DayOfWeek getDayOfWeek();
   ```

### 4.2.3 更改器方法与访问器方法

1. 更改器方法：调用此方法后，对象的状态会发生改变
2. 访问器方法：只访问对象，不修改对象的方法

> CalendarTest程序，代码参看Chapter4 / src / Unit4_1

## 4.3 用户自定义类

### 4.3.1 自定义Employee类

java中，最简单的类定义如下：

```java
class ClassName
{
	field_1;
    field_2;
    ...
        
    constructor_1;
    constructor_2;
    ...;
    
    method_1;
    method_2;
    ...
}
```

下面介绍一个非常简单的Employee类：

（在IDEA里面，可以使用**Alt+Insert**键快速选择构建构造函数和getter和setter方法）

```java
public class Employee {
    //instance fields   实例域
    private String name;
    private double salary;
    private LocalDate hireDay;

    //constructors  构造函数
    public Employee(String name, double salary, int year, int month, int day) {
        this.name = name;
        this.salary = salary;
        this.hireDay = LocalDate.of(year, month, day);
    }
    //无参构造函数
    public Employee() { }

    //methods   方法
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }

    public LocalDate getHireDay() {
        return hireDay;
    }

    public void setHireDay(LocalDate hireDay) {
        this.hireDay = hireDay;
    }
    public static void main(String[] args) {
        Employee[] staffs = new Employee[3];
        staffs[0] = new Employee("1号", 2000, 2000, 12, 20);
        staffs[1] = new Employee("2号", 2000, 2000, 12, 20);
        staffs[2] = new Employee("3号", 2000, 2000, 12, 20);

        for (Employee e : staffs) {
            System.out.println("name = " + e.getName() + " , salary = " + e.getSalary()
                    + " , hireDay = " + e.getHireDay());
        }
    }
}
```

1. 如果将不带public修饰Employee类和带public修饰的EmployeeTest类（包含main函数）放在一个java文件中，javac之后会产生Employee.class和EmployeeTest.class两个文件，之后再使用字节码解释器运行带main函数的.class文件：java EmployeeTest

### 4.3.2 多个源文件的使用

一般大的工程，是会将单个类和主要的main函数分开的，即放置在不同的java文件中。但是整个工程项目只有一个main函数入口，其他单个的类可以使用main函数，但是只是充当单个类的测试。

一个工程如果其main函数入口的java文件为EmployeeTest.java，当使用javac命令进行编译EmployeeTest.java时，会自动也编译里面使用的Employee.java文件。

> 注意：规范的文件使用方式是**一个java源文件只存放一个类**

### 4.3.3 剖析Employee类

1. 实例域一般强烈建议设置为**private**，因为需要实现封装，如果使用public，其他类可以直接访问和修改该类的实例域数据，这是不安全的，破坏了封装性；
2. 方法一般设置为**public**，意味着任何类的任何方法都可以调用这些方法；

### 4.3.4 从构造器开始

1. 构造器可以重载，即构造器可以使用不同参数列表实现不同功能来构造对象；

2. 构造器与其他方法有一个重要的不同：构造器总是伴随着new操作符的执行被调用，而不能对一个已经存在的对象调用构造器来达到重新设置实例域的目的，例如：

   ```java
   	james.Employee("James",200000,,2000,6,12);	//将会产生编译错误
   ```

3. 需要记住如下几点：

   - 构造器与类同名
   - 每个类可以有一个以上的构造器
   - 构造器可以有0，1或多个参数
   - 构造器没有返回值
   - 构造器总是伴随着new操作一起调用

4. 注意：java构造对象是在堆中构建的，类似c++堆上创建指针对象，必须使用new，c++程序员最容易将new遗忘掉；

5. 不要在构造器中定义与实例域重名的局部变量（但是构造器传入的参数一般习惯和实例域命名一致，如下）。

   ```java
   public class Student
   {
   	private String name;
       private String number;
       
       public Student(String name, String number)
       {
           this.name = name;
           this.number = number;
       }
   }
   ```

   

### 4.3.5 隐式参数与显式参数

1. 隐式参数：调用方法的类对象（在一个类中，每个方法的隐式参数为**this**）

2. 显式参数：方法名后面括号中的数值

   ```java
   	//隐式参数：james
   	//显式参数：5
   	james.rasieSalary(5);
   ```

3. 在c++中，类方法在类外部定义，如果在类内部定义方法，这个方法将被自动设置为内联（inline）方法；

   在java中，所有的方法都必须在类的内部定义，但并不表示他们是内联（inline）方法。是否将某个方法设置为内联方法是Java虚拟机的任务。即时编译器会监视调用那些简洁、经常被调用、没有被重载以及可优化的方法。

### 4.3.6 封装的优点

在java中，使用封装屏蔽了类的具体实现，使得类的数据更加安全

java中的类一般需要提供以下三项内容：

- 私有的数据域
- 公有的域访问器方法（getter）
- 公有的域更改器方法（setter）

好处:

1. 可以改变类的内部实现，除了该类的方法外，不会影响其他代码
2. 更改器方法可以执行错误检查，然而直接对域进行赋值将不会进行这些处理，听起来就很糟糕

> **警告：**不要编写返回引用可变对象的访问器方法，如果需要返回一个可变对象的引用，首先应该对它进行clone（）。具体内容参看P110

### 4.3.7 基于类的访问权限

从前面已经知道，方法可以访问所调用对象的私有数据。一个方法可以访问所属类的所有对象的私有数据（这里需要注意以下普通变量和static变量的区别）

### 4.3.8 私有方法

1. 我们知道，应该将所有的数据域都设置为私有的
2. 一般的方法设置为公有的
3. 如果方法是为了某个公有方法的计算的辅助方法，则它不需要在公有接口中出现，所以将其设置为private方法。如果算法改进，不再需要此private方法，可以将其删除

### 4.3.9 final实例域

可以将实例域定义为final。**构建对象时必须初始化这样的域**。

即必须确保在每一个构造器执行之后，这个域的值被设置，并且在后面的操作中，不能再对它进行修改

1. final修饰符大都应用于基本类型域，或不可变类的域（如果类中每个方法都不会改变其对象，这种类就是不可变的类，例如 String 类 ）

2. final关键字只是表示存储在变量中的对象引用不会再指示其他类对象，但是这个对象可以更改（基本数据类型及其包装类当然不能修改，因为包装类没有域更改器），如下：

   ```java
   public class Employee
   {
       ...;
       private final StringBuilder evaluations;
      	...;
       
       public Employee()
       {
   		evaluations = new StringBuilder();
       }
       
       public void giveGoldStar()
       {
   		evaluations.append(LocalDate.now() + ": Gold star!\n");
       }
   }
   ```

   

## 4.4 静态域与静态方法

*定义：属于类但不属于类对象的变量和函数*

更多内容参看：[ java static关键字]([https://peachluis.github.io/2019/04/08/java%E4%B8%ADstatic%E7%9A%84%E9%83%A8%E5%88%86%E7%90%86%E8%A7%A3/](https://peachluis.github.io/2019/04/08/java中static的部分理解/))

### 4.4.1 静态域

1. 如果将一个域定义为static，则每个类中只有这么一个这样的域，而每一个对象对于所有的实例域却都有自己的一份拷贝。例如下面代码：**每一个Student对象都有自己的commonID域，但这个类的所有实例（对象）将共享一个staticID域。如果没有类对象，普通域不存在，但是静态域存在，静态域属于类，而不属于任何独立的对象**换句话说，如果有1000个Student类对象，则有1000个实例域commonID，但是只有一个静态域staticID。

   ```java
   class Student
   {
   	private static int staticID =1;
       private int commonID;
   }
   ```

### 4.4.2 静态常量

使用频率：静态变量（少）

​					静态常量（常），如`Math.PI`，`System.out`

所有创建的对象都共享类中的静态常量，可以使用 **“类名.常量名”** 的方式来得到这个常量值，如`Math.PI`

```java
public Math
{
    ...;
    public static final double PI = 3.14159265358979323846;
    ...;
}

	System.out.print(Math.PI);
```

### 4.4.3 静态方法

1. 普通方法使用 **“对象名.方法名”** 来进行方法调用
2. 静态方法使用 **“类名.静态方法名”** 来进行静态方法调用，也可以使用使用对象名来调用静态方法，但是不建议这样做，因为容易造成混淆。可以认为静态方法是没有this参数的方法
3. **注意：**静态方法不能访问普通变量实例域，只能访问静态域；但是，普通方法静态域和实例域都可以访问
4. 静态方法适用场景：
   - 一个方法不需要访问对象状态，其所需参数都是通过显式参数提供（如：`Math.pow`）
   - 一个方法只需要访问类的静态域（如上`Student.getStaticID`）

### 4.4.4 工厂方法

1. 工厂方法全称**静态工厂方法**，如`LocalDate.now`，`LocalDate.of`，还有NumberFormat类使用工厂方法生成不同的格式化对象：

   ```java
   NumberFormat currencyFormatter = NumberFormat.getCurencyInstance();
   NumberFormat percentFormatter = NumberFormat.getPercentInstance();
   double x = 0.1;
   System.out.println(currencyFormatter.format(x));
   System.out.println(percentFormatter.format(x));
   ```

2. 为什么使用工厂方法原因：

   - 无法命名构造器。构造器的名字必须与类名相同。但是，这里希望将得到的货币实例和百分比实例采用不同的名字。
   - 当使用构造器时，无法改变所构造对象的类型。而工厂方法将返回一个DecimalFormat类对象，这是NumberFormat的子类

### 4.4.5 main方法

1. main方法不对任何对象进行操作。事实上，在启动程序时还没有任何一个对象。静态的main方法将执行并创建程序所需要的对象。
2. 每个类都可以有一个main方法，这是一个常用于对类进行单元测试的技巧。单元测试该类就只使用java命令编译运行该类，运行整个程序就使用java命令编译并运行程序入口的java文件，其他单元测试的类的main函数不会运行。

## 4.5 方法参数

1. 先介绍下程序设计语言中关于**按值调用（call by value）**和**按引用调用（call by reference）**

   - 按值调用：方法接收的是调用者提供的值

   - 按引用调用：方法接受的是调用者提供的变量地址

     注意：一个方法可以修改**传递引用** 所对应的变量值，而不能修改**传递值**调用的所对应的变量值

2. **在java中，总是采用按值调用。**也就是说，方法得到的是所有参数值的一个拷贝，特别是，方法不能修改传递给它的任何参数变量的内容。

3. 然而方法参数有两种类型：

   - 基本数据类型（数值型，布尔值）

   - 对象引用

     **这里需要特别注意：**一个方法不可能修改一个基本数据类型的参数。但对于对象引用就不同了，因为方法得到的是对象引用的拷贝，对象引用及其他的拷贝同时引用了同一个对象。具体参看书上三个例子 P118。

4. 还有一点需要注意，c++中提供两种参数传递方式：值调用和引用调用。有些人认为java中对对象采用的是引用调用，所以java中不能像c++那样通过引用实现两个对象的交换函数`swap`实际上，java还是采取的值调用，具体参看p119例子

5. 总结一下java中方法参数使用情况：

   - 一个方法不能修改一个基本数据类型的参数（布尔值或数值型）
   - 一个方法可以改变一个对象参数的状态
   - 一个方法不能让对象参数引用一个新的对象

## 4.6 对象构造

### 4.6.1 重载

1. 定义：多个方法名字相同，参数类型不同
2. 方法的签名：方法名+参数表（返回类型不是方法签名的一部分，也就是说，**不能有两个名字相同、参数类型也相同却返回不同类型值的方法**）

### 4.6.2 默认域初始化

1. 首先我们需要知道java中的默认初始化

   ```java
   //1.基本方法创建一个数组
   int[] a;	//声明
   a = new int[3];	//使用new构造
   for(int i=0;i<3;i++)	//依次将整型数组初始化为0
   {
   	a[i] = 0;
   }	
   
   //2.简单的初始化一个整型数组,此方法一行代码等效于上面方法，数组中所有元素被默认赋值为0
   int[] a = new int[3];
   
   ```

2. 如果在构造器中没有显式地给域赋予初值，那么域就会被自动赋值为默认值，即数值型为0，布尔值为false，对象引用为null。但是，如果这样的话影响可读性，**一般情况下，都必须在构造器中给域赋值**。

3. 方法中局部变量与类域变量的区别：

   - 方法中的局部变量必须明确初始化，不初始化，编译无法通过
   - 域变量可以不初始化，值被自动赋值为默认值，但一般情况下都需要在构造器中明确初始化

### 4.6.3 无参数的构造器

1. 当一个类没有提供构造函数时，系统会自动提供无参数的构造函数，这个默认构造函数会将实例域变量都初始化为默认值；

2. 只有当一个类没有提供构造函数时，系统默认构造函数才存在；而如果一个类有一个有参数的构造函数，在构造对象的时候却采取无参构造函数构造对象，系统会报错：

   ```java
   	Student a = new Student();	//Student类没有提供构造函数，系统提供默认构造函数来构造对象
   	Student a = new Student();	//Student类提供了有参构造函数，没有提供无参构造函数，再使用无参构造，系统会报错
   ```

### 4.6.4 显式域初始化

1. 可以在类定义中，直接将一个值赋给任何域，这个值不一定是常量值，也可以是调用类方法对域进行初始化；
2. 执行顺序如下：先执行域变量的赋值操作，再执行构造器的内容；
3. 在c++中，不能直接初始化类的实例域，所有的域必须在构造器中设置

### 4.6.5 参数名

方法的参数名，不同的程序员有不同的风格，主要有以下几种：

1. 使用单个字母（简单，但可读性差，不推荐）；

2. 使用参数的具体意思的英文单词（可读性强，推荐），在构造器中，常用如下方法：

   ```java
   public Student(String name, int age)
   {
       this.name = name;
       this.age = age;
   }
   ```

### 4.6.6 调用另一个构造器

this关键字的用处：

1. 作为引用方法的隐式参数；

2. 使用this（）语句，调用同一个类的另一个构造器，采用这种方式对公共的构造器代码部分只需编码一部分即可，如下：

   ```java
   public Student(String name)
   {
       //调用Student(String,int)
       this(name,20);		//注意：这里的this必须放在该构造器的第一行语句
   }
   ```

### 4.6.7 初始化块

这里总结以下初始化数据域的方法：

1. 在构造器中设置值

2. 在数据域后面直接声明赋值

3. 初始化块赋值，如下：

   下面这个代码执行顺序为:

   1. static语句块
   2. main函数
   3. 如果构造一个对象，则调用如下：
      1. 初始化块
      2. 构造器

   ```java
   public class BlockTest {
       private String name;
       private int age;
   
       {
           System.out.println("初始化块");
       }
   
       static {
           System.out.println("静态初始化块");
       }
   
       public BlockTest(String name, int age) {
           this();
           System.out.println("有参构造函数");
           this.name = name;
           this.age = age;
       }
   
       public BlockTest() {
           System.out.println("自定义无参构造函数");
       }
   
       public static void main(String[] args) {
           System.out.println("main主函数");
           BlockTest a = new BlockTest();
           BlockTest b = new BlockTest("name", 15);
       }
   
   }
   
   ```

注意：初始化块不常用，但是使用时需注意，**建议将初始化块放在域定义之后，避免循环定义**

由于初始化数据域有多种途径，所以列出构造过程的所有路径可能相当混乱。下面是调用构造器的具体处理步骤：

1. 所有的数据域被初始化为默认值（0，0.0，false或null）
2. 按照在类声明中出现的次序，依次执行所有域初始化语句和初始化块
3. 如果构造器第一行调用了第二个构造器，则执行第二个构造器主体
4. 然后执行该构造器主体

如果对类的静态域进行初始化的代码比较复杂，可以将其代码放置在static块（静态的初始化块）中

## 4.7 包

1. java允许使用包（package）将类组织起来。借助于包可以方便地组织自己的代码，并将自己的代码和别人提供的代码库分开管理；
2. 标准的java类库分布在多个包中，包括java.lang、java.util、java.net等。标准的java包具有一个层次结构。如同硬盘的目录嵌套一样，也可以使用嵌套层次组织包。所有标准的java包都处于java和javax包层次中；
3. 使用包的**主要原因是确保类名的唯一性**。事实上，为了保证包名的绝对唯一性，Sun公司建议将公司的因特网域名（这显然是独一无二的）以逆序的形式作为包名。例如horstmann.com，包名则为com.horstmann，这个包还可以进一步划分为子包，如com.horstmann.corejava；
4. 从编译器的角度看，嵌套的包之间没有任何关系。例如，java.util包与java.util.jar包毫无关系，每一个都拥有独立的类集合。

### 4.7.1 类的导入

一个类可以使用所属包中的所有类，以及其他包中的公有类（public），我们可以采取两种方式访问另外一个包中的公有类：

1. 法一：在每个类名之前添加完整的包名，例如：

   ```java
   	java.time.localDate today = java.time.LocalDate.now()
   ```

   此方法繁琐，但是能明确指出所导入的类，不常用

2. 法二：使用import语句导入一个特定的类或者整个包，而无须在前面加上包前缀，import语句应该位于源文件顶部（但位于package语句的后面）,

   ```java
   import java.util.*;
   
   LocalDate today = LocalDate.now();
   ```

   可以导入一个包的特定类，如：import java.util.LocalDate;

   也可以使用导入整个包，如：import java.util.*;

   此方法语法简单，常用；

   但是，需要注意的是，只能使用星号导入一个包，而不能使用import java.*或者 import java.星号.星号导入以java为前缀的所有包，这一点看过jdk文件下的这些包的结构就明白了。

   在大多数情况下，只需导入所需的包，并不必过多地理睬他们。但在发生命名冲突的时候，就不能不注意包的名字了。例如 java.util 和 java.sql 包都有Date类，如果在程序中导入者两个包，编译器就无法识别到底调用的是哪个包的Date类，可以采用增加一个特定的import语句来解决，或者明确指出包名

   ```java
   //法一：增加一个特定的import语句
   import java.util.*;
   import java.sql.*;
   import java.util.Date;
   
   Date a = new Date();
   
   //法二：指出明确的包名
   java.util.Date a = new java.util.Date();
   java.sql.Date a = new java.sql.Date(..);
   ```

   注意：java中的import语句与c++中的include并不类似，而是与c++中的命名空间（namespace）类似

### 4.7.2 静态导入

静态导入，导入静态方法和静态域

```java
//使用静态导入，但对于特定的熟悉的类，比如说System.out,System.exit不利于代码的清晰度
import static java.util.System.*;

out.print("Hello!");
```

### 4.7.3 将类放入包中

要想将一个类放入包中，就必须将包的名字放在源文件的开头，包中定义类的代码之前，如果没有在源文件中放置package语句，这个源文件中的类就被放置在一个默认包中（default package）中。例如：

```java
package com.horstmann.corejava;

public class Student
{
	...
}
```

将包中的文件放到与完整的包名匹配的子目录中。例如，com.horstmann.corejava包中的所有源文件应该被放置在子目录com/horstmann/corejava中。编译器将类文件也放在相同的目录结构中。

如果包与目录不匹配，虚拟机就找不到类，虽然可以编译，但是程序无法正常运行。

### 4.7.4 包作用域

1. public可以被任意的类使用；
2. private只能被定义他们的类使用；
3. protected只能是该包内或者其父子类使用
4. default只能是包内使用

一般变量必须显式地标记为private，不然的话缺省值将默认为包可见，这样会破坏封装性。在java.awt包中的Window类中部分域例如String warningString值为缺省值，这就意味着java.awt包中的所有类的方法都可以访问该变量并修改它，因此应该将它设置为私有变量。这个问题至今还没有得到纠正，不仅如此，这个类还增加了一些新域，其中大约一般也不是私有的。

从1.2版本开始，JDK的实现者修改了类加载器，明确地禁止加载用户自定义的、包名以“java.”开始的类。但是，用户自定义的类无法从这种保护中受益。然而，可以通过包密封（package sealing）机制来解决各种包混杂在一起的问题。如果将一个包密封起来，就不能再向这个包添加类了。在第9章中，将介绍制作包含密封包的jar文件的方法。

## 4.8 类路径

我们已经知道，类存储在文件系统的子目录中。类的路径必须与包名匹配。

另外，类文件也可以存储在 JAR 文件（java归档）文件中。在一个JAR文件中，可以包含多个压缩形式的类文件和子目录，这样既可以节省空间又可以改善性能。在程序中用到第三方库文件时，通常会给出一个或多个需要包含的JAR文件。JAR文件使用ZIP格式组织文件和子目录。

为了使类能够被多个程序共享（例如jdk的使用），需要做到下面几点：

1. 把类放到一个目录中，例如 /home/user/classdir。需要注意，这个目录是包树状结构的基目录。如果希望将com.horstmann.corejava.Employee 类添加到其中，这个**Employee.class**类文件就必须位于子目录 /home/user/classdir/com/horstmann/corejava 中。

2. 将jar文件放在同一目录中，例如：/home/user/archives。

3. 设置类路径（class path）。类路径使所有包含类文件的路径的集合。

   在UNIX环境中，类路径中的不同项目之间采用冒号（：）分隔；

   而在Windows环境中，则以分号（；）分隔：

   c:\classdir;.;c:\archives\archive.jar

   句点（.）表示当前目录。

   类路径包括：

   - 基目录c:\classes；
   - 当前目录（.）；

   - JAR文件 c:\archives\archive.jar；

   从 java SE 6开始，可以在jar文件目录中指定通配符，如下：

   ​	c:\classdir;.;c:\archives\*

   在archives目录中的所有 JAR 文件（但不包括.class文件）都包含在类路径中。

   由于运行时库文件（ rt.jar 和在 jre/lib 与 jre/lib/ext 目录下的一些其他的 JAR 文件）会被自动地搜索，所以不必将它们显式地列在类路径中。

   > **警告：**javac编译器总是在当前的目录中查找文件，但java虚拟机仅在类路径中有 ”.“ 目录的时候才查看当前目录。如果没有设置类路径，那也不会产生声明问题，默认的类路径包含 ”.“ 目录。然而如果设置了类路径却忘记了包含 ”.“ 目录，则程序仍然可以通过编译，但不能运行。

   类路径所列出的目录和归档文件总是搜寻类的起始点

总的来说：

- JVM通过环境变量`classpath`决定搜索`class`的路径和顺序；
- 不推荐设置系统环境变量`classpath`，始终建议通过`-cp`命令传入；
- java -classpath一般用在命令行操作的时候，在IDEA，Eclipse等当中不需要你配置，系统会自动配置

> 更多内容参看[廖雪峰的教程](https://www.liaoxuefeng.com/wiki/1252599548343744/1260466914339296)

## 4.9 文档注释

jdk中的一个工具——javadoc：由源文件生成一个HTML文档。

在源代码中添加以 /**开始的注释，形成 javadoc 。

如果将文档存入一个独立的文件中，可能随着时间的推移，出现代码和注释不一致。然而，由于文档注释与源代码在同一个文件中，在修改源代码的同时，重新运行 javadoc 就可以轻而易举地保持两者的一致性。

### 4.9.1 注释的插入

javadoc 从下面几个特性中抽取信息：

- 包
- 公有类与接口
- 公有的和受保护的构造器和方法
- 公有的和受保护的域

每个文档注释在标记之后紧跟着自由格式文本。标记由@开始，如@author或@param。

自由格式文本第一句应该是一个概要性的句子。Javadoc实用程序自动将这些句子抽取出来形成概要页。

在自由格式文本中，可以使用HTML修饰符，例如：

```html
<em>用于强调</em>
<strong>用于着重强调</strong>
<img src=...>图像

不能使用，因为会与文档的格式产生冲突：
<h1>...</h1>
<hr>...</hr>

输入等宽代码：
因为会产生代码中<>转义不能使用：<code>...</code>
需要使用：{@code ...}
```

### 4.9.2 类注释

类注释必须放在import语句之后，类定义之前。

### 4.9.3 方法注释

每一个方法注释必须放在所描述的方法之前。除了通用标记之外，还可以使用下面的标记：

- @param变量描述

  这个标记将对当前方法的 “param” （参数）部分添加一个条目。这个描述可以占据多行，并可以使用HTML标记。一个方法的所有@param标记必须放在一起。

- @return描述

  这个标记将对当前方法添加 “return” （返回）部分。这个描述可以跨越多行，并可以使用HTML标记。

- @throw类描述

  这个标记将添加一个注释，用于表示这个方法有可能抛出异常。有关异常的详细内容将在第10章讨论

### 4.9.4 域注释

只需要对公有域（通常指的是静态常量）建立文档。

### 4.9.5 通用注释

下面的标记可以用在**类文档的注释**中：

- @author 姓名

  这个标记将产生一个 “author”（作者）条目。可以使用多个@author标记，每个标记对应一个作者。

- @version 文本

  这个标记将产生一个 "version"（版本）标目。这里的文本可以是对当前版本的任何描述

下面的标记可以用于所有的文档注释中：

- @since 文本

  这个标记将产生一个 “since”（始于）条目。这里的文本可以是对引入特性的版本描述。例如：@since version 1.7.1

- @deprecated 文本

  这个标记将对类、方法或变量添加一个不再使用的注释。文本中给出了取代的建议。例如：

  ```java
  @deprecated Use <code> setVisible(true) </code> instead
  ```

- @see 引用

  这个标记将在 “see also” 部分增加一个超链接。它可以用于类中，也可以用于方法中。如：

  ```java
  @see com.horstmann.corejava.Employee#raiseSalary(double)
  //如果是在该包下，可以省略包名；在该类中，可以省略类名
  //需要注意，一定要使用（#），而不要使用句号（.）分隔类名与方法名，或类名与变量名
  ```

  如果@see 标记后面有一个 ”<“ 字符，就需要指定一个超链接。可以链接到任何URL。例如：

  ```java
  @see <a href="www.horstmann.com/corejava.html">The Core Java home page</a>
  ```

  在上述各种情况下，都可以指定一个可选的标签（lable）作为链接锚。如果省略了lable，用户看到的锚的名称就是目标代码或URL。

  如果@see标记后面有个双引号字符，文本就会显式在双引号中部分，例如：

  ```java
  @see "Core Java volum 2"
  ```

  可以为一个特性添加多个@see标记，但必须将它们放在一起

- 如果愿意的话，还可以在注释中的任何位置放置指向其他类或方法的超级链接，以及插入一个专用的标记，例如：

  ```java
  {@link package.class#featrue lable}
  //这里的特性描述规则与@see标记规则一样
  ```

### 4.9.6 包与概述注释

要产生包注释，需要在每一个包目录中添加一个单独的文件。可以有2中选择：

1. 提供一个以package.html命名的HTML文件。在标记<body>...</body>之间的所有文本都会被抽取出来
2. 提供一个以package-info.java命名的java文件。这个文件必须包含一个初始的以/**标示的 Javadoc 注释，跟随在一个包语句之后。它不应该包含更多的代码或注释。

还可以为所有的源文件提供一个概述性的注释，这个注释被放置在一个名为overview.html的文件中，这个文件位于包含所有源文件的父目录中。标记<body>...</body>之间的所有内容将被抽取出来。

### 4.9.7 注释的抽取

。。。

## 4.10 类设计技巧

1. 一定要保证数据私有
2. 一定要对数据初始化
3. 不要在类中使用过多的基本类型
4. 不是所有的域都需要独立的域访问器和域更改器
5. 将职责过多的类进行分解
6. 类名和方法名要能够体现它们的职责
7. 优先使用不可变的类

# 第五章 继承

利用继承，人马可以基于已存在的类构造一个新类。继承已存在的类就是复用（继承）这些类的方法和域。在此基础上，还可以添加一些新的方法和域，以满足新的需求。

反射是指在程序运行期间发现更多的类及其属性的能力。

## 5.1 类、超类、子类

注释：在这一章中，我们使用员工和经理的传统示例，不过必须提醒你对这个例子要有所保留。在真是世界里，员工也可能会成为经理，所以你建模时可能希望经理也是员工，而不是一个子类。不过，在我们的例子中，假设公司里只有两类人：一些人一直是员工，另一些人一直是经理。

### 5.1.1 定义子类

使用extends关键字，所有的继承都是公有继承：

```java
public class Manager extends Employee
{
    ...
}
```

关键字extends表明正在构造的新类派生于一个已存在的类。已存在的类称为超类（superclass）（常用）、基类（base class）或父类（parent class）；新类称为子类（subclass）（常用）、派生类（derived class）或孩子类（child class）。

子类比超类拥有的功能更加丰富。

**超类不能使用子类定义的方法和域；子类拥有超类的所有域和方法，但是在子类只有构造器中使用super（）访问超类的构造器的这种方法能访问访问超类的private数据域，其他方法中不能访问到超类中的private数据域和方法**

在通过扩展超类定义子类的时候，仅需指出子类与超类的不同之处。因此在设计类的时候，应该将通用的方法放在超类中，而将具有特殊用途的方法放在子类中。

### 5.1.2 覆盖方法（即重写override）

在`Manager`和`Employee`类的例子中：

`Manager`类的`getSalary（）`方法不能直接访问超类的私有域（但能访问非public域）。也就是说，尽管每个`Manager`对象都拥有一个名为`salary`的域，但在`Manager`类的`getSalary（）`方法并不能直接地访问私有域。只有`Employee`类的方法才能够访问私有部分。如果`Manager`类的方法一定要访问私有域，就必须借助于公有的接口，`Employee`类中的公有方法`getSalary（）`正是这样的一个接口。

**在子类中调用超类的非私有方法和域，使用`super`关键字，格式为：`super.域名`，`super.方法名()`。**

> 注释：有些人认为`super`与`this`引用是类似的盖帘，实际上，这样比较并不太恰当。这是因为`super`不是一个对象的引用，不能将`super`赋给另一个对象变量，它只是一个指示编译器调用超类方法的特殊关键字。

在子类中可以增加域、增加方法或覆盖超类的方法，然而绝对不能删除继承的任何域和方法。

### 5.1.3 子类构造器

```java
public Manager(String name,double salary,int year,int month,int day)
{
    super(name,salary,year,month,day);
    bonus = 0;
}
```

这里的super关键字具有另外的含义：即调用超类的相同参数的构造函数的简写形式

需要记住：子类中的构造器必须包含有超类中的构造器，子类构造器中第一条语句也必须是使用`super`关键字传入超类构造器（在超类中有无参构造函数时，子类构造函数可以不包括其他超类构造函数，也不用使用super关键字，但是这是java默认的省略，实际上这些构造器都是包含自超类的无参构造函数内容），如下：

```java
public class Person
{
	private String name;
    
    public Person(String name)
    {
		this.name = name;
        System.out.println("这是Person（String name）构造器");
    }
    
    public Person()
    {
        System.out.print("这是Person（）构造器");
    }
}

class Student extends Person
{
	private int num;
    
    public Student(String name, int num)
    {
		super(name);
        this.num = num;
    }
    
    public Student(int num)
    {
		this.num = num;
    }
    
    public static void main(String[] args)
    {
		Student a = new Student("小王",2000);
        Student b = new Student(1200);
    }
}

//输出结果：
这是Person（String name）构造器
这是Person（）构造器
```

**使用super调用构造器的语句必须是子类构造器的第一条语句。**

如果子类构造器没有显式地调用超类的构造器，则将自动地调用超类默认（没有参数）的构造器。如果超类没有不带参数的构造器，并且在子类的构造器中又没有显式地调用超类的其他构造器，则java编译器将报告错误。

> `this`关键字两个用途：
>
> 1. 引用隐式参数（当作隐身的对象）
> 2. 调用该类的其他构造器
>
> `super`关键字两个用途：
>
> 1. 调用超类的非私有方法和域
> 2. 调用超类的构造器
>
> 注意：两个关键字相似，在调用构造器时，都必须作为构造器的第一条语句

一个对象变量可以指示多种实际类型的现象称为**多态**，在运行时能自动地选择调用哪个方法的现象称为**动态绑定**

> 在 Java 中，不需要将方法声明为虚拟方法（c++中将多态方法声明为虚拟方法）。动态绑定是默认的处理方式。如果不希望一个方法具有虚拟特性，可以将它标记为`final`。

### 5.1.4 继承层次

由一个公共超类派生出来的所有类的集合被称为**继承层次**，如p154所示，在继承层次中，从某个特定的类到其祖先的路径被称为该类的**继承链**。通常，一个祖先类可以有多个子孙继承链，它们彼此之间没有关系。

> java不支持多继承：即A既继承于B，又继承于C；java采取的方法是接口

### 5.1.5 多态

有一个用来判断是否应该设计为继承关系的简单规则，就是 ”is-a" 规则，它表明子类的每个对象也是超类的对象。

“is-a” 规则的另一种表述法是置换法则：程序中出现超类对象的任何地方都可以用子类对象置换。可以将一个子类的对象赋给超类变量，但是注意：绝对不能将超类变量赋给子类变量。

在 Java 中，对象变量是多态的。一个`Employee`变量既可以引用`Employee`类对象，也可以引用一个`Employee`类的任何一个子类的对象。

> 警告⚠：在 Java 中，子类数组的引用可以转换成超类数组的引用，而不需要采用强制类型转换，如：
>
> ```java
> Manager[] managers = new Manager[10];
> Employee[] staffs = managers;	//OK
> ```
>
> 具体更多内容参看 P155

### 5.1.6 理解方法调用

下面假设要调用 x.f(args)，隐式参数x声明为类C的一个对象，下面是调用过程的描述：

1. 编译器查看对象的声明类型和方法名。编译器将一一列举C类中名为f的方法和其超类中访问类型为public且名为f的方法。（超类的私有方法不可访问）

   至此，编译器已获得所有可能被调用的候选方法。

2. 编译器将查看调用方法时提供的参数类型。如果在所有名为 f 的方法中存在一个与提供的参数类型完全匹配，就选择这个方法。这个过程称为**重载解析**。如果编译器没有找到与参数类型匹配的方法，或者发现经过类型转换后有多个方法与之匹配，就会报告一个错误。

   至此，编译器已获得需要调用的方法名字和参数类型（即签名）。

   > 方法名字+参数类型 = 签名。但返回类型不属于签名。如果子类中定义了一个与超类签名相同的方法，则称子类方法覆盖超类方法。
   >
   > 允许子类将覆盖方法的返回类型定义为原返回类型的子类型（对于自定义类来说就是子类），如：
   >
   > ```java
   > public Employee getBuddy(){...}
   > 
   > public Manager getBuddy(){...}
   > ```
   >
   > 我们称这个两个getBuddy（）方法具有可协变的返回类型

3. 如果是private方法、static方法、final方法或者构造器，那么编译器将可以准确地知道应该调用哪个方法，我们将这种调用方式称为**静态绑定**。与此对应的是，调用方法依赖于隐式参数的实际类型，并且在运行时实现的**动态绑定**。

4. 当程序运行，并且采用动态绑定调用方法时，虚拟机一定调用了于与x所引用对象的实际类型最合适的那个类的方法。

   > 每次调用方法时都要进行搜索，时间开销相当大。因此，虚拟机预先为每个类创建了一个方法表（method table），其中列出了所有的方法的签名和实际调用的方法。这样一来，在真正调用方法时，虚拟机仅查找这个表就可以了。

   动态绑定有一个非常重要的特性：无需对现存的代码进行修改，就可以对程序进行扩展。

   > 警告⚠：在覆盖一个方法的时候，子类方法不能低于超类方法的可见性。特别是，如果超类方法是public，子类方法一定要声明为public。经常发生这类错误：在声明子类方法的时候，遗漏了public修饰符。此时，编译器将会把它解释为试图提供更严格的访问权限。

### 5.1.7 阻止继承：final类和方法

1. 有时候，可能希望阻止人们利用某个类定义子类。不允许扩展的类被称为final类。如果在定义类的时候使用了final修饰符就表示这个类是fianl类。

2. 类中的特定方法也可以被声明为final。如果这样做，子类就不能覆盖这个方法（fianl类中的所有方法自动地称为final方法，但不包括域）。

3. 将方法或类声明为final主要目的是：确保它们不会在子类中改变语义。
4. 如果一个方法没有被覆盖而且很短，编译器就能对它进行优化处理，这个过程称为内联（inline）。然而，如果被覆盖，那么编译器就无法知道覆盖的代码将会做什么操作，因此就不能对它进行内联处理了。
5. 幸运的是，虚拟机中的即时编译器比传统编译器的处理能力强得多。这种编译器可以准确地知道类之间的继承关系，并能够检测出类中是否真正地覆盖给定的方法。如果方法很简短、被频繁调用且没有真正地被覆盖，那么既时编译器就会将这个方法进行内联处理，如果虚拟机加载了另一个子类，而在这个子类中包含了对内联方法的覆盖，那么将会发生什么情况呢？优化器将取消对覆盖方法的内联。这个过程很慢，但却很少发生。

### 5.1.8 强制类型转换

1. 超类转子类会出现一个ClassCastException，需要加上`（）`强转；子类转超类不需要加上强转。
2. 进行类型转换的唯一原因：在暂时忽视对象的实际类型之后，使用对象的全部功能。

> 一个简单的记忆方式是：
>
> - 大范围的类（子类）转小范围的类（超类）就相当于切割掉子类的一部分，但是超类的信息完全保留，直接转换即可；
> - 小范围的类（超类）转大范围的类（子类）范围会变大，但是子类的扩展内容超类都没有，这些内容都会是空白，会引起异常，所以需要加上强制转换

### 5.1.9 抽象类

在类的继承层次结构当中，越往上的类就越具有通用性，这就是我们说的*越抽象*

要创建一个抽象类，只需要加上`abstract`即可，如`public abstract class Person`；

抽象方法没有body，所以抽象方法以分号`;`结尾；

> 抽象方法充当着占位的角色，它们的具体实现在子类中。
>
> 扩展抽象类可以有两个选择：
>
> - 一种是在抽象子类中定义部分抽象类方法或不定义抽象类方法，这样就必须将子类也标记为抽象类；
> - 另一种是定义全部的抽象方法，这样一来，子类就不是抽象的了；

注意**声明**和**定义**的区别，不然会将上面内容混淆。

所以上面的话简单来说就是：**一个类继承了一个抽象类就要实现抽象类中的所有抽象方法；若不能全部实现，则这个类就必须为抽象类；**

注意：

- 抽象类中可以包含非抽象方法，这些非抽象方法可以有自己的具体实现
- 类即使不含抽象方法，也可以将类声明为抽象类
- 抽象类不能被实例化；但是可以定义一个抽象类的对象变量，但是它只能应用非抽象子类的对象，如`Person p = new Student("小王",123456);`（这里Person是抽象类，Student继承自Person类，但是非抽象）

### 5.1.10 受保护访问

1. 在java的使用中，一种不成文的规定为：最好将类中的域标记为`private`，方法标记为`public`。任何声明为private的内容对其他类都不可见，对子类也一样。

2. 有的时候，对于超类中的不可见域和方法，人们可能希望子类能访问到，所以有了`protected`关键字；

   但是，`protected`对域作用会违反OOP提倡的数据封装原则，不建议使用；

   `protected`修饰的方法却更具有实际意义，这表明子类（可能很熟悉祖先类）得到信任，可以正确地使用这个方法，而其他类则不行；一个最好的示例就是`Object`类中的`clone`方法；

3. 归纳以下java中用于控制可见性的4个访问修饰符：

   - 仅对本类可见——`private`
   - 对所有类可见——`public`
   - 对本类和所有子类可见——`protected`
   - 对本包可见——默认（很遗憾），不需要修饰符

## 5.2 Object：所有类的超类

`Object`类是Java中所有类的始祖，在Java中每个类都是由它扩展而来的。

熟悉这个类提供的所有服务十分重要。

在Java中，只有基本类型不是对象，但是所有的数组类型，无论是对象数组还是基本类型数组都扩展了Object类。

### 5.2.1 equals方法

`Object`类中的`equals`方法用于检测一个对象是否等于另一个对象，但是只是简单的判断引用是否相等`return (this == obj)`；但是实际用处不大，所以一般自定义的类进行判断都会重写`equals`方法

```java
//java.util.Object
public boolean equals(Object boj)
{
	return (this == obj);
}

//java.util.Objects
public static bool equals(Object a, Object b)
{
    return (a == b) || (a != null && a.equals(b));
}
```



# 赞助

如果这篇博客对你有帮助，或许你可以请我喝杯咖啡

<center class="half">
    <img src="https://raw.githubusercontent.com/PeachLuis/Picture_bed/master/img/zhifubao_ma.jpg
" width="250"/>
    <img src="https://raw.githubusercontent.com/PeachLuis/Picture_bed/master/img/wechat_ma.jpg
" width="250"/>
</center>



