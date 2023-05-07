# HIT-软件构造实验3-2023春



# 1 实验目标概述

编写具有可复用性和可维护性的软件，主要使用以下软件构造技术：子类型、泛型、多态、重写、重载；继承、委派、CRP；语法驱动的编程、正则表达式；设计模式。

本次实验给定了多个具体应用，不是直接针对每个应用分别编程实现，而是通过 ADT 和泛型等抽象技术，开发一套可复用的 ADT 及其实现，充分考虑这些应用之间的相似性和差异性，使 ADT 有更大程度的复用（可复用性）和更容易面向各种变化（可维护性）

# 2 实验环境配置

在IDEA创建新项目，创建test文件夹并设置成测试文件夹，添加.gitignore，创建git仓库并链接到远程仓库

仓库的URL：https://github.com/ComputerScienceHIT/HIT-Lab3-2021112946 

# 3 实验过程

请仔细对照实验手册，针对每一项任务，在下面各节中记录你的实验过程、阐述你的设计思路和问题求解思路，可辅之以示意图或关键源代码加以说明（但千万不要把你的源代码全部粘贴过来！）。

## 3.1 待开发的三个应用场景

列出要完成的具体应用场景

l 行星运动模拟（StellarSystem）

l 原子结构模型（AtomStructure）

l 社交网络的好友分布（SocialNetworkCircle）

分析你所选定的多个应用场景的异同，理解需求：它们在哪些方面有共性、哪些方面有差异。

## 3.2 基于语法的图数据输入

使用gitSSH链接下载输入示例文件

git clone [git@github.com:rainywang/Spring2019_HITCS_SC_Lab3.git](mailto:git@github.com:rainywang/Spring2019_HITCS_SC_Lab3.git)

基本思路：使用正则表达式解析输入内容

在abstract class ConcreteCircularOrbit<L,E>中定义方法readData完成这个功能。

1） 为了处理不同的数据，定义抽象方法readCenter(String srcStr)，readPhysic(String srcStr)具体分析数据，供readData调用

2） 由于读数据有共性，readData在抽象类ConcreteCircularOrbit中就进行实现：读取文件中的所有内容，并传给上述两个抽象方法。

3） 由于使用正则表达式解析内容具有共性，在抽象类ConcreteCircularOrbit中定义并实现match(String reg, String str)封装了正则表达式解析，供readCenter(String srcStr)，readPhysic(String srcStr)调用

## 3.3 面向复用的设计：CircularOrbit<L,E>

定义接口CircularOrbit<L,E>，在接口内声明了要求的所有方法。

抽象类ConcreteCircularOrbit<L,E>实现接口CircularOrbit<L,E>，规定req

```
//中心点物体
L center;
//轨道和轨道上的物体
Map<Track,List<E>> tracks_map=new HashMap<>();
//中心点轨道物体关系
Map<E,Integer> relationship=new HashMap<>();
//规定读取数据的分隔符
protected static final String delimiter=",";
```

实现所有方法，新增了抽象方法readCenter(String srcStr)，readPhysic(String srcStr)

## 3.4 面向复用的设计：Track

Track的设计比较简单

```
private Number radius;

public Track(Number radius) {
    this.radius = radius;
}

/**
 * 获取
 * @return radius
 */
public Number getRadius() {
    return radius;
}
```

并提供getter和setter

## 3.5 面向复用的设计：L

抽象类centralObject中提取共性

```
protected String name;
```

Atom

```
public class Atom extends CentralObject{
    //private String Name;

    public Atom(String name) {
        this.name=name;
    }
}
```

centerUser

```
public class CentralUser extends CentralObject{
    private String Name;
    private int age;
    private char gender;
}
```

Stellar

```
public class Stellar extends CentralObject{

    //private String name;
    private Number radius;
    private Number quality;

    public Stellar(String name, Number radius, Number quality) {
        this.name = name;
        this.radius = radius;
        this.quality = quality;
    }
}
```

 

## 3.6 面向复用的设计：PhysicalObject

抽象类PhysicalObject中提取了共性req

```
//和其他轨道物体之间的关系
private Map<E,Double> relationshipMap=new HashMap<>();
//位置
private double position;
```

## 3.7 可复用API设计

熵的计算

```
/**
 * 计算多轨道系统中各轨道上物体分布的熵值
 * @param c
 * @return
 */
double getObjectDistributionEntropy(CircularOrbit c){
    //计算多轨道系统中各轨道上物体分布的熵值
    //-∑(p*log p)
    List l=c.getObjectNumber();
    float sum=0;
    double result=0;
    for(Object o:l){
        Integer i=(Integer)o;
        sum+=i;
    }
    for(Object o:l){
        Integer num=(Integer)o;
        result+=(num/sum)*Math.log(num/sum);
    }
    return result;
}
```

 

## 3.8 图的可视化：第三方API的复用

 

## 3.9 设计模式应用

请分小节介绍每种设计模式在你的ADT和应用设计中的具体应用。

## 3.10 Git仓库结构

请在完成全部实验要求之后，利用Git log指令或Git图形化客户端或GitHub上项目仓库的Insight页面，给出你的仓库到目前为止的Object Graph，尤其是区分清楚312change分支和master分支所指向的位置。

 

# 4 实验进度记录

 

请使用表格方式记录你的进度情况，以超过半小时的连续编程时间为一行。

| 日期      | 时间段      | 计划任务                                             | 实际完成情况                                               |
| --------- | ----------- | ---------------------------------------------------- | ---------------------------------------------------------- |
| 已轶      | 已轶        | 阅读理解题目内容                                     | 基本理解，未能设计完成                                     |
| 2023/4/5  | 下午        | 中心物体、轨道物体、轨道、系统的抽象类和一部分实现类 | 最开始的设计有些不合理，只完成了一部分；                   |
| 2023/4/5  | 晚上        | 同上                                                 | 遇到了很多bug，成功读取并构建了太阳系                      |
| 2023/4/20 | 19:11~20:30 | 完成三种系统的文件读取                               | 完成了太阳系和原子系统的读取。社交网络由于多级结构未能完成 |
| 2023/5/3  | 13:00~17:30 | 完成可复用API的编码                                  | 完成                                                       |

# 5 实验过程中遇到的困难与解决途径

| 遇到的难点                               | 解决途径                                                     |
| ---------------------------------------- | ------------------------------------------------------------ |
| 正则表达式非捕获分组仍然被捕获           | 没完全弄明白，似乎java会捕获被标记为非捕获分组的内容。由于读取的内容一定是第二个分组（第一个非捕获分组），所以暂时用.groud(2)捕获到第二个分组（0是整个捕获内容） |
| 大数字读取失败                           | 原因：超出double范围。解决：Req里的类型改成Number，实际使用时可以传入BigDecimal |
| 使用TreeSet保存Track和轨道上的物体时报错 | 原因：TreeSet的key必须实现Comparable接口；方法1：Track实现Comparable接口；方法2：改用HashMap。由于Track里采用了Number，不同数据类型难以直接比较大小，故采用方法2； |

# 6 实验过程中收获的经验、教训、感想

## 6.1 实验过程中收获的经验和教训

 

## 6.2 针对以下方面的感受

(1)  重新思考Lab2中的问题：面向ADT的编程和直接面向应用场景编程，你体会到二者有何差异？本实验设计的ADT在五个不同的应用场景下使用，你是否体会到复用的好处？

a)    差异：面向ADT要考虑不同应用场景中的共性并提取出共有的方法

b)   好处：提高了代码的复用性、可维护性

(2)  重新思考Lab2中的问题：为ADT撰写复杂的specification, invariants, RI, AF，时刻注意ADT是否有rep exposure，这些工作的意义是什么？你是否愿意在以后的编程中坚持这么做？

a)    意义：代码可读性

b)   尽量

(3)  之前你将别人提供的API用于自己的程序开发中，本次实验你尝试着开发给别人使用的API，是否能够体会到其中的难处和乐趣？

不能

(4)  在编程中使用设计模式，增加了很多类，但在复用和可维护性方面带来了收益。你如何看待设计模式？

(5)  你之前在使用其他软件时，应该体会过输入各种命令向系统发出指令。本次实验你开发了一个解析器，使用语法和正则表达式去解析输入文件并据此构造对象。你对语法驱动编程有何感受？

(6)  Lab1和Lab2的大部分工作都不是从0开始，而是基于他人给出的设计方案和初始代码。本次实验是你完全从0开始进行ADT的设计并用OOP实现，经过三周之后，你感觉“设计ADT”的难度主要体现在哪些地方？你是如何克服的？

a)    难处：由于要实现的内容较为复杂，设计时

(7)  你在完成本实验时，是否有参考Lab4和Lab5的实验手册？若有，你如何在本次实验中同时去考虑后续两个实验的要求的？

否

(8)  关于本实验的工作量、难度、deadline。

大难早

(9)  到目前为止你对《软件构造》课程的评价。

任务多，截止时间早，其他还好

 