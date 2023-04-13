











![image-20230303111848249](HIT_SC_lab1.assets/9ePyxfh3LbjqlG7.png)

## HIT软件构造2023年春Lab1 Turtle Graphics (MIT)

理解：题目提供了绘制图形轨迹的代码集，我们需要按要求对其中`TurtleSoup.java`的几个方法进行方法体的编写

#### 在项目中导入junit

打开就看到junit包没正确导入

![image-20230303093535247](HIT_SC_lab1.assets/v59WTsq3gJOyPhp.png)

打开文件--项目结构--模块--添加JAR或目录

![image-20230303094211830](HIT_SC_lab1.assets/yLDcgw295NTftn4.png)

在`安装目录\lab\`下找到junit4.jar，导入即可

![image-20230303094346375](HIT_SC_lab1.assets/lpKWe9M6qI7SbVz.png)

现在再看就不是暗红的了

![image-20230303094556853](HIT_SC_lab1.assets/iKRjLQv9T53C8NE.png)



![image-20230303092458544](HIT_SC_lab1.assets/bJrC2KcNEH9YFPO.png)

#### 问题3

![image-20230303094815952](HIT_SC_lab1.assets/JmMncLHbl3zUps1.png)

为`public static void drawSquare(Turtle turtle, int sideLength)`编写绘制正方形的代码

```java
for(int i=0;i<4;i++){
    turtle.forward(sideLength);
    turtle.turn(90);
}
```

运行`TurtleSoup.java.main`

![image-20230303095414328](HIT_SC_lab1.assets/lirpOGzJyxb4wLE.png)

#### 问题5

为`public static double calculateRegularPolygonAngle(int sides)`编写方法体

```java
return ((double)sides-2)*180.0/(double)sides;
```

调试运行`TurtleSoupTest`，看到`calculateRegularPolygonAngleTest`通过了

![image-20230303101526228](HIT_SC_lab1.assets/TIYduD5p4CU3BzN.png)

为`public static void drawRegularPolygon(Turtle turtle, int sides, int sideLength)`编写方法体

```java
double degree=180-calculateRegularPolygonAngle(sides);
for(int i=0;i<sides;i++){
    turtle.forward(sideLength);
    turtle.turn(degree);
}
```

修改`main`中调用的方法

```java
DrawableTurtle turtle = new DrawableTurtle();
//drawSquare(turtle, 40);
drawRegularPolygon(turtle,6,50);
// draw the window
turtle.draw();
```

运行`main`

![image-20230303102900306](HIT_SC_lab1.assets/YS18zLkjd2MvZoD.png)

#### 问题6

为`public static double calculateBearingToPoint(double currentBearing, int currentX, int currentY, int targetX, int targetY)`编写方法体

```java
double degree = 90-Math.toDegrees(Math.atan((double) (targetY-currentY)/(double)(targetX-currentX)))-currentBearing;
if(degree<0)degree+=360;
return degree;
```

调试运行`TurtleSoupTest`

![image-20230303104303882](HIT_SC_lab1.assets/uANlXBvzyiEpnFV.png)

为`public static List<Double> calculateBearings(List<Integer> xCoords, List<Integer> yCoords)`编写方法体

```java
List<Double> list=new ArrayList<>();
double currentBearing=0;
int i=0;
int listLength=xCoords.size();
while (i+1<listLength){
    double degreeChange=calculateBearingToPoint(currentBearing, xCoords.get(i),yCoords.get(i), xCoords.get(i+1),yCoords.get(i+1) );
    list.add(degreeChange);
    currentBearing=(currentBearing+degreeChange)%360;
    i++;
}
return list;
```

调试运行`TurtleSoupTest`

![image-20230303110221822](HIT_SC_lab1.assets/McOpx8gYD4aNVuL.png)





#### 问题8

为`public static void drawPersonalArt(Turtle turtle)`编写方法体

```java
turtle.color(PenColor.YELLOW);
//turtle.forward(100);
turtle.turn(270);
turtle.turn(180-(double)6*180/16);
for(int i=1;i<200;i++){
    turtle.forward(i);
    turtle.turn(180-(double)6*180/8);
    turtle.color(i%2==0?PenColor.BLUE:PenColor.YELLOW);
}
turtle.turn(180+(double)6*180/8);
turtle.turn(90);
turtle.forward(5);
turtle.turn(90);
for(int i=1;i<200;i++){
    turtle.forward(200-i-1);
    turtle.turn(180+(double)6*180/8);
    turtle.color(i%2==0?PenColor.BLUE:PenColor.YELLOW);
}
```

