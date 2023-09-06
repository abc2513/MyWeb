# SVG笔记

SVG是一种基于XML的图像格式（可缩放矢量图），是对图像形状描述的文本

可以直接插入网页 。也可以写到单独文件里，用img（不能操作）、iframe、object、embed等标签+src属性 插入

- 矢量图，不会失真
- 直接作为元素插入页面，相对于图片可以进行操作

## 语法

### `svg`标签

顶层标签，所有SVG代码都放在此标签内

- `width` ,`height`：宽度、高度
- `viewbox`：切出视窗两个顶点的横纵坐标px，在需要显示部分内容时使用

```html
<svg width='100px' height='100px' viewBox='50 50 100 100'>
    <circle cx='50' cy='50' r='50'/>
</svg>
```

==各种标签，可以使用css的各种属性，包括动画等==

### `circle`圆形标签

代表圆形

- cx,cy：圆形坐标x、y
- r：半径
- fill：填充颜色
- stroke：描边颜色
- stroke-width
- 可以使用style/class等属性，将相关属性写到样式内

<svg>
    <circle cx="100" cy="80" r="50" stroke="blue" stroke-width="5" fill="lightblue" />
</svg>

### `line`线段标签

代表线段

- x1 y1 x2 y2

```html
<svg>
    <line x1="0" y1="0" x2="200" y2="100" stroke="black" stroke-width="2" />
</svg>
```

<svg>
    <line x1="8" y1="8" x2="200" y2="100" stroke="blue" stroke-width="5" />
</svg>

### `polyline`折线标签

代表折线

- points

```html
<svg>
    <polyline points="10,10 100,10 100,100 200,100" stroke="black" stroke-width="2" fill="transparent" />
</svg>
```

<svg>
    <polyline points="10,10 100,10 100,100 200,100" stroke="purple" stroke-width="5" fill="transparent" />
</svg>

### `reat`矩形标签

- x,y
- height,width

<svg>
    <rect x='10' y='10' height='100' width='200' fill='yellowgreen' stroke='green' stroke-width='5'/>
</svg>

### `ellsipse` 椭圆标签

- cx,cy
- rx,ry

<svg>
    <ellipse cx='120' cy='60' rx='100' ry='50' fill='orange' stroke='brown' stroke-width='5'/>
</svg>

polygon 多边形

- points

<svg>
    <polygon points='10,10 10,100 200,100' fill='pink' stroke='red' stroke-width='5'/>
</svg>

### `path` 路径

- d：绘制顺序
  - M移动到(m)
  - L画直线到(l)
  - Z闭合路径(z)
  - 贝塞尔曲线、圆弧 等命令

```html
<svg width='100%'>
    <path d='M 10,50 L 120,50 M 150,50 L 250,150 L 350,50 Z' stroke='green' stroke-width='3' fill='none'/>
</svg>
```

<svg width='100%'>
    <path d='M 10,50 L 120,50 M 150,50 L 250,150 L 350,50 Z' stroke='green' stroke-width='3' fill='none'/>
</svg>

### `text` 文本标签

- x,y
- children
- 字体的颜色是用fill填充，

```html
<svg>
    <text x='50' y='50'  fill='yellowgreen'>hello_world</text>
    <text x='80' y='80'  fill='yellowgreen'>hello_world</text>
    <text x='110' y='110'  fill='yellowgreen'>hello_world</text>
</svg>
```

<svg>
    <text x='50' y='50'  fill='yellowgreen'>hello_world</text>
    <text x='80' y='80'  fill='yellowgreen'>hello_world</text>
    <text x='110' y='110'  fill='yellowgreen'>hello_world</text>
</svg>
### `use`复制标签

```html
<svg width='100%'>
    <circle id='circle_1' cx="100" cy="80" r="50" stroke="blue" stroke-width="5" fill="lightblue" />
    <use href='#circle_1' x='20' y='10' fill='orange' stroke='brown'/>
</svg>
```




### `g`组标签

用于把多个标签组合，便于一起操作

```html
<svg width='100%' height='300px'>
    <g id='group'>
        <circle cx='100' cy='100' r='30'/>
        <circle cx='200' cy='100' r='30'/>
        <circle cx='300' cy='100' r='30'/>
    </g>
    <use href='#group' x='30' y='100'/>
</svg>
```

### `pattern`标签



