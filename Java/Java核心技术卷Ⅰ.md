# Java核心技术卷Ⅰ

## 第1章 Java程序设计概述

> *`Java`是一个完整的平台，有一个庞大的库，其中包含了很多可重用的代码和一个提供诸如安全性、跨操作系统的可移植性以及自动垃圾收集等服务的执行环境。*

### Java“白皮书”的关键术语

1. 简单性
    1. `Java`语法是`C++`的一个”纯净“版本。
    2. 简单的另一个方面是小。`Java`的目标之一是支持开发能够在小型机器上独立运行的软件。
2. 面向对象
3. 分布式
4. 健壮性
5. 安全性
6. 体系结构中立
    1. 编译器生成一个体系结构中立的目标文件格式（字节码），这是一种编译过的代码，只要有`Java`运行时系统，这些编译后的代码可以在许多处理器上运行。
    2. 解释虚拟机指令肯定会比全速运行机器指令慢很多，然而`JVM`有一个选项，可以将执行最频繁的字节码序列翻译成机器码，这一过程被称为及时编译。
7. 可移植性
8. 解释性
9. 高性能
10. 多线程
11. 动态性
    1. 在`Java`中找出运行时类型信息十分简单

### Java发展历史

| 版本     | 发布时间 | 名称      |
| -------- | -------- | --------- |
| JDK Beta | 1995     | WebRunner |
|JDK 1.0 |1996.1| Oak|
|  JDK 1.1| 1997.2|  |
|J2SE 1.2| 1998.12 |Playground|
|  J2SE 1.3 |2000.5 |Kestrel |
| J2SE 1.4 |2002.2 |Merlin |
| J2SE 5.0 |2004.9| Tiger |
| Java SE 6 |2006.12| Mustang|
|  Java SE 7| 2011.7|Dolphin|
|  Java SE 8 (LTS)| 2014.3 ||
| Java SE 9 |2017.9||
| Java SE 10 (18.3) |2018.3||
|  Java SE 11 (18.9 LTS)| 2018.9 ||
| Java SE 12 (19.3)| 2019.3||
|  Java SE 13 (19.9)| 2019.9||
|  Java SE 14 (20.3)| 2020.3||

## 第2章 Java程序设计环境

### 使用命令行工具

```shell
javac Welcome.java
java Welcome
```

## 第3章 Java的基本程序设计结构

### 注释

* 单行注释 `//`

* 多行注释 `/* */`

    * 不支持嵌套

* 文档注释 `/** */`

    * ```java
        /**
         * This is the first sample program in Core Java Chapter 3
         * @version 1.0
         * @author dk
         */
        public class FirstSample{
          public static void main(String[] args){
            System.out.println("Hello, World")
          }
        }
        ```

### 数据类型

8种基本数据类型

* 4种整型
    * byte   1字节  8位    -2^7 ~ 2^7 - 1
    * short  2字节 16位  -2^15 ~ 2^15 - 1
    * int       4字节 32位  -2^31​ ~ 2^31- 1 （21亿）
    * long    8字节 64位  -2^63 ~ 2^63 - 1 后缀加L
    * 十六进制  0x
    * 八进制      0
    * 二进制      0b
    * Java没有任何无符号类型（unsigned），kotlin有
* 2种浮点类型
    * float      4字节 1位符号位 8位指数位（决定大小） 23位小数位（决定精度）  有效位数6-7位 后缀加F
    * double  8字节 1位符号位 11位指数位 52位小数位 有效位数15位  后缀可以加D，不强制，不加D默认浮点数值类型也是double类型
* 1种字符类型
    * char  2字节  范围从\u0000到\uffff
* 1种布尔类型
    * boolean true false 整型值和布尔值之间不能互转

### 常量

* final修饰 一旦赋值，就不能再更改了
* (public) static final 类常量

### 运算符

* 整数被0除会产生一个异常，而浮点数被0除会得到无穷大或者NaN结果

* floorMod()和%区别

    * ```java
        //如果参数的符号相同
        floorMod(4, 3) == 1;   and (4 % 3) == 1
        ```

    * ```java
        //如果参数的符号不同
        floorMod(+4, -3) == -2;   and (+4 % -3) == +1
        floorMod(-4, +3) == +2;   and (-4 % +3) == -1
        floorMod(-4, -3) == -1;   and (-4 % -3) == -1
        ```

* 数值类型转换

    * 如果一个数是double类型，另一个操作数就会转换成double类型
    * 否则如果一个数是float类型，另一个操作数就会转换成float类型
    * 否则如果一个数是long类型，另一个操作数就会转换成long类型
    * 否则两个数都会将转换成int类型

* 强制类型转换

    * 浮点数类型转换成整数类型，会截断小数部分

        * ```java
            double a = 9.999;
            int b = (int)a;
            
            b = 9
            ```

    * 四舍五入可以使用Math.round()方法

### 字符串

* 当一个字符串一一个非字符串的值进行拼接的时候，后者被转换成字符串

* String.equals()比较两个字符串值是否相等，==比较两个字符串是否在同一个位置上

* jdk5.0 引入StringBuilder类，这个类的前身是StringBuffer，其效率稍有些低，但允许采用多线程的方式之慈宁宫添加或删除字符的操作。如果所有字符串在一个单线程中编辑，则应该使用StringBuilder替代它。这两个类的API是相同的。

* 格式化字符串 printf

    * 转换符

        | 转换符 |         类型         |   举例   |
        | :----: | :------------------: | :------: |
        |   d    |      十进制整数      |   123    |
        |   x    |     十六进制整数     |    9f    |
        |   o    |      八进制整数      |   257    |
        |   e    |      指数浮点数      | 1.59e+01 |
        |   g    |      通用浮点数      |    -     |
        |   a    |    十六进制浮点数    | 0x1.fccd |
        |   s    |        字符串        |  Hello   |
        |   c    |         字符         |    H     |
        |   b    |         布尔         |   True   |
        |   h    |        散列码        |  456416  |
        | tx或Tx |       日期时间       |    -     |
        |   %    |        百分号        |    %     |
        |   n    | 与平台有关的行分隔符 |    -     |
        |   f    |      定点浮点数      |   15.9   |

    * 标志

        |        标志        |                             目的                             |     举例     |
        | :----------------: | :----------------------------------------------------------: | :----------: |
        |         +          |                     打印正数和负数的符号                     |   +333.33    |
        |        空格        |                       在正数之前加空格                       | \|  333.33\| |
        |         0          |                          数字前补0                           |   00333.33   |
        |         -          |                            左对齐                            | \|333.33  \| |
        |         (          |                       将负数括在括号内                       |  （333.33）  |
        |         \.         |                        添加分组分隔符                        |  33,333.33   |
        |  \#（对于f格式）   |                          包含小数点                          |    3,333.    |
        | \#（对于x或0格式） |                        添加前缀0x或0                         |    0xab12    |
        |         $          | 给定被格式化的参数索引，例如%1\$d,%1\$x将以十进制和十六进制打印第一个参数 |    159 9F    |
        |         <          | 格式化前面说明的数值，例如%d%<x以十进制和十六进制打印同一个数值 |    159 9F    |

        可以使用多个标志
        
    * 日期转换符
    
        * * 
    
        | 转换符 | 类型                                                    | 举例                         |
        | ------ | ------------------------------------------------------- | ---------------------------- |
        | c      | 完整的日期和时间                                        | Mon Feb 09 18:05:11 PST 2004 |
        | F      | ISO 8061日期                                            | 2004-02-09                   |
        | D      | 美国格式的日期                                          | 02/09/2004                   |
        | T      | 24小时时间                                              | 18:03:23                     |
        | r      | 12小时时间                                              | 05:03:23 pm                  |
        | R      | 24小时时间没有秒                                        | 18:05                        |
        | Y      | 4位数字的年（前面补0）                                  | 2004                         |
        | y      | 年的后两位数字（前面补0）                               | 04                           |
        | C      | 年的前两位数字（前面补0）                               | 20                           |
        | B      | 月的完整拼写                                            | February                     |
        | b或h   | 月的缩写                                                | Feb                          |
        | m      | 两位数字的月（前面补0）                                 | 02                           |
        | d      | 两位数字的日（前面补0）                                 | 09                           |
        | e      | 两位数字的月（前面不补0）                               | 9                            |
        | A      | 星期几的完整拼写                                        | Monday                       |
        | a      | 星期几的缩写                                            | Mon                          |
        | j      | 三位数的年中的日子（前面补0），在001到366之间           | 069                          |
        | H      | 两位数字的小时（前面补0），在0到23之间                  | 18                           |
        | k      | 两位数字的小时（前面不补0）在0到23之间                  | 8                            |
        | I      | （大写的i）	两位数字的小时（前面补0），在0到12之间   | 06                           |
        | l      | （小写的L）	两位数字的小时（前面不补0），在0到12之间 | 6                            |
        | M      | 两位数字的分钟（前面补0）                               | 05                           |
        | S      | 两位数字的秒（前面补0）                                 | 19                           |
        | L      | 三位数字的毫秒（前面补0）                               | 047                          |
        | N      | 九位数字的毫微秒（前面补0）                             | 047000000                    |
        | P      | 上午或者下午的大写标志                                  | PM                           |
        | p      | 上午或者下午的小写标志                                  | pm                           |
        | z      | 从GMT起，RFC822数字移位                                 | -0800                        |
        | Z      | 时区                                                    | PST                          |
        | s      | 从格林威治时间1970-01-01 00:00:00起的秒数               | 107884319                    |
        | Q      | 从格林威治时间1970-01-01 00:00:01起的毫秒数             | 107884319047                 |



### 控制流程

* break

    * 用于退出循环语句

    * 用于退出嵌套循环

        ```java
        tag:
        for (int i = 0; i < 10; i++) {
          for (int j = 0; j < 10; j++) {
            if (j == 2)
              break tag;
          }
        }
        ```



### 大数值

* BigInteger 整数

    ```java
    BigInteger a = BigInteger.valueOf(100);
    BigInteger b = a.add(BigInteger.valueOf(20));
    BigInteger c = b.multiply(a);
    ```

* BigDecimal 浮点数

### 数组

* 初始化

```java
int[] array = new int[]{100, 100};
int[] array1 = {100, 100};
int[] array2 = new int[0];
System.out.printf("%s %s %s", array.length, array1.length, array2.length);

2 2 0
```

在Java中，允许数组长度为0，但是数组长度为0与null不同

* 数组拷贝

    ```java
    // 引用拷贝
    int[] luckyNumbers = samllPrimes;
    luckyNumbers[2] = 90; //luckyNumbers[2] is also 90
      
    // 值拷贝
    int copiedLuckyNumbers = Arrays.copyOf(luckyNumbers, luckyNumbers.length);
    // 第二个参数是新数组长度，通常用来增加数组的大小，如果是数值型，那么多余的数组被赋值为0，布尔型被赋值为false。相反如果长度小于原来数组的长度，则只会拷贝前面的数据元素
    luckyNumbers = Arrays.copyOf(luckyNumbers, luckyNumbers.legth * 2);
    ```

* 数组排序

    `Arrays.sort(luckyNumbers)` 改变原数组

* 多维数组

    `double[][] blances`

* 不规则数组

## 第4章 对象与类

### 面向对象程序设计概述

* 面向对象程序设计 OOP(Object Oriented Program)

* 在OOP中，不必关心对象的具体实现，只要满足用户的需求即可

* 类（class）是构造对象的模板或蓝图。由类构造（construct）对象的过程称为创建类的实例（instance）

* 对象中的数据称为实例域（instance field），操作数据的过程称为方法（method）

* 实现封装的关键在于绝对不能让类中的方法直接地访问其他类的实例域

* 在扩展一个类的时候，这个扩展后的心累具有所扩展类的全部属性和方法，在新类中只需提供适合这个新类的方法和数据域就可以了

* 类与类之间的常见关系

    * 依赖（uses-a）

        如果一个类的方法操纵另一个类的对象，我们就说一个类依赖另一个类

    * 聚合（has-a）

        一个Order对象包含一些Item对象，聚合关系意味着类A包含类B的对象

    * 继承（is-a）

### 使用预定义类

* 一个对象变量并没有实际包含一个对象，而仅仅是引用了一个对象，在Java中，任何对象变量的值都是对存储在另外一个地方的一个对象的引用

* LocalDate 用来处理日历

    ```java
    LocalDate today = LocalDate.now();
    int somedays = today.getDayOfMonth() - 1;
    int month = today.getMonthValue();
    LocalDate firstDay = today.minusDays(somedays);
    
    System.out.println("Mon Tue Wed Thu Fri Sat Sun");
    for (int i = 0; i < firstDay.getDayOfWeek().getValue() - 1; i++) {
      System.out.print("    ");
    }
    while (firstDay.getMonthValue() == month) {
      System.out.printf("%3d", firstDay.getDayOfMonth());
    
      if (firstDay.getDayOfMonth() == today.getDayOfMonth()) {
        System.out.print("*");
      } else {
        System.out.print(" ");
      }
    
      if (firstDay.getDayOfWeek() == DayOfWeek.SUNDAY) {
        System.out.println();
      }
    
      firstDay = firstDay.plusDays(1);
    }
    ```

### 用户自定义类

```java
class Employee{
  // instance fields
  private String name;
  private double salary;
  private LocalDate hireDay;
  
  // constructor
  public Employee(String n, double s, int year, int month, int day){
    name = n;
    salary = s;
    hireDay = LocalDate.of(year, month, day);
  }
  
  // a method
  public String getName(){
    return name;
  }
  
  // more methods
}
```

* 构造器

    * 构造器和类同名
    * 每个类可以有一个以上的构造器
    * 构造器可以有0个、1个或多个参数
    * 构造器没有返回值
    * 构造器总是伴随着new操作一起调用

* 隐式参数和显示参数

    ```java
    public void raiseSalary(double byPercent){
      double raise = salary * byPercent / 100;
      salary += raise
    }
    ```

    * raiseSalary方法有两个参数。第一个参数称为隐式（implicit）参数，即方法名前面出现的Employee类对象。第二个参数位于方法名后面括号中的数值，这是一个显示（explicit）参数。（有人把隐式参数称为方法调用的目标或者接受者）在每一个方法中，关键字this表示隐式参数

* 不要编写返回引用可变对象的访问器方法，如果需要返回一个对象的引用，应该首先对它进行克隆（clone）

    ```java
    class Employee{
      public Date getHireDay(){
        return (Date)hireDay.clone();
      }
    }
    ```

* final实例域

    * 可以将实例域定义为final。构建对象时必须初始化这样的域。也就是说，必须确保在每一个构造器执行之后，这个域的值被设置，并且后面的操作中，不能再对它进行修改

### 静态域与静态方法

* 静态域
    * `private static int indexId = 1 `它属于类，而不属于任何独立的对象
* 静态常量
    * `public static final double PI = 3.141592653589793`
* 静态方法
    * 静态方法是一种不能向对象实施操作的方法。
    * `Math.pow(x, a)`在运算时，不使用任何Math对象。换句话说，没有隐式参数
    * 可以认为静态方法是没有this参数的方法
    * 对象可以调用静态方法，但是不推荐，原因是静态方法的计算结果和对象毫无关系，容易造成混淆，建议使用类名调用
    * 下面两种情况下使用静态方法
        * 一个方法不需要访问对象状态，其所需参数都是通过显示参数提供（如：Math.pow）
        * 一个方法只需要访问类的静态域
    * 工厂方法
        * LocalDate.now
        * LocalDate.of
        * 为什么不利用构造器完成这些操作
            * 无法命名构造器
            * 当使用构造器时，无法改变所构造的对象类型

### 方法参数

按值调用（call by value）表示方法接收的是调用者提供的值

按引用调用（call by reference）表示方法接收的是调用者提供的变量地址

一个方法可以修改传递引用所对应的变量值，而不能修改传递值调用所对应的变量值

**Java程序设计语言总是采用按值调用。也就是说，方法得到的是所有参数值的一个拷贝，特别是，方法不能修改传递给它的任何参数变量的内容**

很多程序设计语言（特别是C++和Pascal）提供了两种参数传递的方式：值调用和引用调用。有些程序员认为Java程序设计语言对对象采用的是引用调用，实际上，这种理解是不对的。例如：

```java
public static void swap(Employee x, Employee y){
  Employee temp = x;
  x = y;
  y = temp
}

public static void main(String[] args){
  Employee a = new Employee("Tom", 8888, 1990, 1, 1);
  Employee b = new Employee("Jack", 9999, 2000, 1, 2);
  swap(a, b)
}
```

如果Java对对象采用的是按引用调用，那么这个方法实现交换数据的效果，但是方法并没有改变存储在变量a和b中的对象引用。swap方法的参数x和y被初始化为两个对象引用的拷贝，这个方法交换的是这两个拷贝。在方法结束时参数变量x，y被丢弃了，原来变量a和b仍然引用这个方法调用之前所引用的对象

* 一个方法不能修改一个基本数据类型的参数
* 一个方法可以改变一个对象参数的状态
* 一个方法不能让对象参数引用一个新的对象

### 对象构造

* 重载（overloading）

    如果多个方法，有相同的名字、不同的参数，便产生了重载。

    要完整地描述一个方法，需要指出方法名以及参数类型。这叫做方法的签名（signature）。返回类型不是方法签名的一部分，也就是说不能有两个名字相同、参数类型也相同但却返回不同类型值的方法

* 默认域初始化

    * 类中的域可以不初始化，没有初始化类中的域，将会被自动初始化为默认值（0、false、null）
    * 局部变量必须进行初始化

* 无参数的构造器

    * 很多类都包含一个无参数的构造函数，对象由无参数构造函数创建时，其状态会设置为适当的默认值
    * 仅当类没有提供任何构造器的时候，系统才会提供一个默认的构造器

* 调用另一个构造器

    this()

* 初始化块（initialization block）

    `{}`构造类的对象，这些块就会执行

* 对象析构与finalize方法

    finalize方法将在垃圾回收器清除对象之前调用。在实际应用中，不要依赖于使用finalize方法回收任何短缺的资源，这是因为很难知道这个方法什么时候才能够调用

### 包

* 类的导入

    * `java.time.LocalDate today = java.time.LocalDate.now();`

    * ```java
        import java.util.*;
        
        LocalDate today = LocalDate.now();
        ```

    * ```java
        import java.util.LocalDate;
        
        LocalDate today = LocalDate.now();
        ```

* 静态导入

    * `import static java.lang.System`

* 将类放入包中

    * `package com.dk.corejava`

* 包的作用域

    * default修饰的类可以被包内的所有方法访问

### 类路径

* 类存储在文件系统的子目录中，类的路径必须与包名匹配
* 类文件也可以存储在JAR（JAVA归档）文件中（JAR文件使用ZIP格式组织文件和子目录，可以使用所有ZIP应用程序查看JAR文件）

### 文档注释

JDK包含一个很有用的工具，叫做javadoc，它可以由源文件生成一个HTML文档

* javadoc会从下面几个特征中抽取信息：
    * 包
    * 公有类与接口
    * 公有的和受保护的构造器及方法
    * 公有的和受保护的域

* 类注释

    ```java
    /**
     * 一个员工类
     */
    public class Employee{
      
    }
    ```

* 方法注释

    ```java
    /**
     * 给一个员工加薪
     * @param byPercent 加薪的百分比（例如：10就是10%）
     * @return 加薪的数额
     */
    public double raiseSalary(double byPercent){
      double raise = salary * byPercent / 100;
      salary += raise;
      return raise;
    }
    ```

* 域注释

    只需要对公有域（通常指的是静态常量）建立文档

    ```java
    /**
     * 员工的类型：老板
     */
    public static final int BOSS = 1;
    ```

* 通用注释

    @author dk

    @version 1.2

    @since version 1.1

    @deprecated

    @see

### 类设计技巧

* 一定要保证数据私有
    * 这是最重要的，绝对不能破坏封装性。
    * 有时候需要编写一个访问器方法或者更改器方法，但是最好还是保持实例域的私有
* 一定要对数据初始化
    * Java不会对局部变量进行初始化，但是会对对象的实例域进行初始化
    * 最好不要依赖系统的默认值，而是应该县实地初始化所有的数据，具体的初始化方式可以是提供默认值，也可以在所有构造器中设置默认值
* 不要在类中使用过多的基本数据类型
    * 就是说，用其他的类代替多个相关的基本类型的使用
    * 例如：一个Address类代替一个Customer类中的`private String street` `private String city` `private String state` `private int zip`
* 不是所有的域都需要独立的域访问器和修改器
* 将职责过多的类进行分解
* 类名和方法名要能体现它们的职责
* 优先使用不可变的类
    * LocalDate类以及java.time包中的其他类是不可变--没有方法能修改对象的状态。类似plusDays方法并不是更改对象，而是返回状态以及修改的新对象

## 第5章 继承

###  类、超类和子类

* 定义子类

    * ```java
        public class Manager extends Employee{
          //添加域和方法
          private double bonus;
        }
        ```

* 覆盖方法

    子类可以覆盖（override）父类的方法

    ```java
    public double getSalary(){
      double baseSalary = super.getSalary();
      return baseSalary + bonus
    }
    ```

    有些人认为super与this引用是类似的概念，实际上，这样比较并不恰当。这是因为super不是一个对象的引用，不能将super赋给另一个对象变量，它只是一个指示编译器调用超类方法的特殊关键字。

* 子类构造器

    ```java
    public Manager(String name, double salary, int year, int month, int day){
      // 使用super调用构造器的语句必须是子类构造器的第一条语句
      super(name, salary, year, month, day);
      bonus = 0;
    }
    ```

    如果子类的构造器没有显示地调用父类的构造器，则将自动地调用父类默认（没有参数）的构造器。如果父类没有不带参数的构造器，并且子类构造器中又没有显示地调用父类的其他构造器，则Java编译器报错。

* 多态

    有一个用来判断是否应该设计为继承关系的简单规则，这就是“is-a”规则，它表明子类的每个对象也是父类的对象。