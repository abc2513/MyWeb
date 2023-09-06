# JS 进阶 学习笔记

### start

##### const优先的变量声明

const优先，有变化时再用let。var不再使用

##### 获取DOM对象

1、借助css选择器querySelector和querySelectorAll

```js
//返回对象可以直接进行操作

//返回匹配的第一个元素HTMLElement对象
const boxElement1=document.querySelector('div')
const boxElement2=document.querySelector('.box')
const boxElement3=document.querySelector('#div2')
const liElement3=document.querySelector('ul li')
boxElement1.innerText='box1'

//返回匹配的元素NodeListOf<HTMLElement>集合(伪数组，无pop()、push()等数组方法)
const boxListElement=document.querySelectorAll('ul li');;
boxListElement[0].style.color='red'
```

2、其他方法

```js
//不大友好

//根据id获取一个元素
document.getElementById('box1')
//根据标签获取一类元素 (数组)
document.getElementsByTagName('div')
//根据类名获取元素 （数组）
document.getElementsByTagName('box')

//特殊：body
document.body
```



##### 操作元素内容

- innerText：识别文本，不解析标签
- innerHTML：识别标签

##### 操作元素属性

- 常用属性：比如修改href, title, src等

- 样式属性：

  - style属性：style.color/width/height等样式属性
  - className属性：'class1 class2'
  - classList属性(es5+)，使用add等数组方法
    - 添加add：`boxEle.classList.add('box')`
    - 移除remove
    - 切换toggle（有移除、没有就加上）

- 表单属性

  - value
  - type
  - checked（不提倡）

- 自定义属性

  ![image-20230529182406858](JSAPI.assets/image-20230529182406858.png)



##### 定时器-间歇函数

```js
fun(){}
const timer=setInterval(fun,1000)//返回id
clearInterval(timer)
```

### 事件

##### 事件监听

也称绑定事件或者注册事件

- https://developer.mozilla.org/zh-CN/docs/Web/Events
- 三要素：事件源（dom元素）、事件类型（触发方法）、事件调用的函数
- 语法：
  - `事件源元素对象.addEventLister('事件类型',回调函数)`：可绑定多次，拥有更多特性
  - `事件源元素对象.OnClick=fun(){}`：会被覆盖

##### 事件类型

- 鼠标事件：click,mouseenter,mouseleave
- 焦点事件：focus获取焦点, blur失去焦点
- 键盘事件：keydown，keyup
- 文本事件：input

##### 事件对象

存储了事件触发时的相关信息

常用属性：

- type：事件类型
- clintX/clintY：光标相对于浏览器可见窗口左上角的位置
- offsetX/offsetY：光标相对于当前DOM元素左上角的位置
- key：键盘值，不再提倡使用keyCode

##### 环境对象this

函数内部的特殊变量 ，代表函数==运行==所处的环境。谁==调用==函数，this指向谁

```js
function fun(){...}
window.fun();//普通函数由window调用，this为window
button.addEventListener('click',fun)//回调函数，this为button（调用者）
```

箭头函数没有自己的this



##### 回调函数

将函数作为参数传递时，称为回调函数



##### 事件流

  ![image-20230731143143658](JSAPI.assets/image-20230731143143658.png)

![image-20230731143916950](JSAPI.assets/image-20230731143916950.png)

##### 事件捕获与冒泡

![image-20230731144325269](JSAPI.assets/image-20230731144325269.png)

##### 阻止事件流动

![image-20230731144634342](JSAPI.assets/image-20230731144634342.png)



##### 事件解绑

![image-20230731145239384](JSAPI.assets/image-20230731145239384.png)



#####  鼠标over和enter 

![image-20230731145737089](JSAPI.assets/image-20230731145737089.png)

![image-20230731151236612](JSAPI.assets/image-20230731151236612.png)

##### 事件委托

减少注册次数，提高程序性能

原理：事件冒泡，给父元素注册事件，触发子元素的时候会冒泡到父元素身上，从而触发父元素的事件

```html
<ul>
    <li>1</li>
    <li>2</li>
</ul>
<script>
    const ulElement=document.querySelector('ul');
    ul.addEventListener('click',function(event){
        //委托的对象,函数调用者
        this.style.color='red';				//ul
        //真正触发的元素
        event.target.style.color='green';	//li
    })
</script>
```

##### 阻止冒泡

比如阻止默认行为的发生，使用event.preventDefault

![image-20230807104658957](JSAPI.assets/image-20230807104658957.png)





##### 两种页面加载事件

外部资源加载完毕触发的事件'load'

```jsx
//页面所有外部资源加载完毕
window.addEventListener('load',()=>{});
//针对某个资源
img.addEventListener('load',()=>{});
```

html文档加载并解析完毕的事件'DOMContentLoaded'，无需等待样式表和图片等资源加载完毕，速度更快

##### 元素滚动事件

'scoll'，window、document、元素均可加

![image-20230807221233563](JSAPI.assets/image-20230807221233563.png)

##### 页面尺寸事件

'resize'，在窗口尺寸改变时触发

clint不包括边框、滚动条

![image-20230807222153313](JSAPI.assets/image-20230807222153313.png)

eg

![image-20230807222947971](JSAPI.assets/image-20230807222947971.png)



![image-20230808095247230](JSAPI.assets/image-20230808095247230.png)



### 日期对象

##### 实例化

```js
//获得当前时间
const date=new Date();
//获得指定时间
const date_2=new Date('2023-5-24');
```



##### 对象方法

注意很多数都是0开始

![image-20230808100023161](JSAPI.assets/image-20230808100023161.png)

##### 时间戳

方便计算时间差

![image-20230808105753693](JSAPI.assets/image-20230808105753693.png)

![image-20230808110023949](JSAPI.assets/image-20230808110023949.png)

### 节点

元素节点、属性节点、文本节点

![image-20230808130946271](JSAPI.assets/image-20230808130946271.png)



##### 根据关系查找节点

获取父节点`ele.parentNode`返回节点

子节点`ele.children`返回伪数组

上一个兄弟元素节点`ele.previousElementSibling`

下一个兄弟元素节点`ele.nextElementSibling`

##### 新增节点

![image-20230808141934062](JSAPI.assets/image-20230808141934062.png)

##### 克隆节点

![image-20230810203118259](JSAPI.assets/image-20230810203118259.png)





##### 删除节点

必须通过父节点

![image-20230810210258668](JSAPI.assets/image-20230810210258668.png)

![image-20230810210321394](JSAPI.assets/image-20230810210321394.png)

### M端事件与JS插件

![image-20230810210809544](JSAPI.assets/image-20230810210809544.png)





![image-20230810211259137](JSAPI.assets/image-20230810211259137.png)

### Windows对象

##### 浏览器对象模型BOM

![image-20230810212117992](JSAPI.assets/image-20230810212117992.png)



##### JS执行机制

![image-20230810213059847](JSAPI.assets/image-20230810213059847.png)

![image-20230810213110110](JSAPI.assets/image-20230810213110110.png)

![image-20230810213617441](JSAPI.assets/image-20230810213617441.png)

![image-20230810213740666](JSAPI.assets/image-20230810213740666.png)

![image-20230810214209961](JSAPI.assets/image-20230810214209961.png)

![image-20230810214836892](JSAPI.assets/image-20230810214836892.png)

##### location对象

![image-20230810215728550](JSAPI.assets/image-20230810215728550.png)

![image-20230810215748573](JSAPI.assets/image-20230810215748573.png)



##### navigator对象

![image-20230810215917602](JSAPI.assets/image-20230810215917602.png)

##### history对象

![image-20230810220636936](JSAPI.assets/image-20230810220636936.png)

##### 本地存储 

![image-20230814092246009](JSAPI.assets/image-20230814092246009.png)

###### localStorage

![image-20230814092727516](JSAPI.assets/image-20230814092727516.png)



![image-20230814101041266](JSAPI.assets/image-20230814101041266.png)



###### sessionStorage

![image-20230814101639252](JSAPI.assets/image-20230814101639252.png)



### 进阶1

##### 作用域

##### 垃圾回收机制

![image-20230814103126261](JSAPI.assets/image-20230814103126261.png)

![image-20230814103619528](JSAPI.assets/image-20230814103619528.png)





![image-20230826152734135](JSAPI.assets/image-20230826152734135.png)

![image-20230826152905482](JSAPI.assets/image-20230826152905482.png)

##### 闭包

##### 变量提升

##### 箭头函数

写法简短，不绑定this

箭头函数内的this，为上一层作用域的this。对象方法写箭头函数，就指向对象

![image-20230828153852282](JSAPI.assets/image-20230828153852282.png)

解构赋值

foreach遍历数组



### 进阶2

##### 深入对象

###### 三种方式创建对象(构造函数)

```js
//1.字面量方式
const obj_1={a:'a'};
//2.new Object()
const obj_2=new Object();
obj_2.a='a';
//3.构造函数

```

###### 构造函数

使用场景：快速创建多个类似的对象

约定：
1. 函数名大写字母开头
2. 只能通过new 操作符执行
3. 不需要写return，返回值就是实例化的对象（写了也没用）

（new Date(), new Object()也是一种构造函数）吗 

![image-20230828155238978](JSAPI.assets/image-20230828155238978.png)



##### 内置构造函数

### 进阶3

### 进阶4

##### 防抖

防抖：一定时间内，频繁触发事件，只执行最后一次。

比如搜索框搜索输入，要等用户输入操作最后一次才发送请求。如果代码里面存在消耗性能的代码，没有防抖可能造成卡顿。

###### 防抖函数

使用lodash库提供的夯实实现防抖

```js
//program
_.debounce(函数,毫秒数);
//eg
box.addEventListener('mousemove',_.debounce(()=>{},1000));
```



###### 手动实现

核心是利用setTimeout定时器

1. 声明一个定时器变量
2. 每次触发都判断是否有定时器，有的话先清除
3. 开启定时器
4. 定时器回调函数

```js
function myDebounce(fn,t){
    let timer;
    return function(){
        //234
        if(timer) clearTimeout(timer);
        setTimeout(fn(),t);
    };
}
box.addEventListener('mousemove',myDebounce(()=>{},1000));
```



##### 节流

节流throttle：一定时间内，频繁触发事件，只执行一次 

高频事件，如鼠标移动、页面缩放、滚动条滚动

###### 节流函数

```js
_.throttle(fun,wait)
```



###### 手动实现

```js
function myThrottle(fn,t){
    let timer=null;
    return function(){
        if(!timer){
            timer=setTimeout(()=>{
            	fn();
                timer=null;//不能clearTimeout，因为这里定时器还在运作，不能在定时器内部清除定时器
            },t);
        }
        
    }
}
```



![image-20230828152534331](JSAPI.assets/image-20230828152534331.png)