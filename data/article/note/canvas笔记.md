# Canvas

初识

```html
<body>
    <canvas id="canvas" width="800" height="600">
        <!-- 不支持的浏览器会将canvas识别为普通标签识别 -->
        <h3>您的浏览器不支持canvas!请更新浏览器!</h3>
    </canvas>
    <script>
        //1.获取画布
        let canvas = document.getElementById('canvas');
        //2.获取画笔(上下文对象)
        let ctx = canvas.getContext('2d');
        //3.绘制
        ctx.fillRect(0,0,100,100);
    </script>
</body>
```

绘制填充/描边/清除

```js
//1.获取画布
let canvas = document.getElementById('canvas');
//2.获取画笔(上下文对象)
let ctx = canvas.getContext('2d');
//3.绘制
ctx.fillRect(0,0,100,100);//绘制填充矩形,位置(0,0)宽高(100,100)
ctx.strokeRect(100,100,100,100);//绘制描边矩形
ctx.clearRect(50,50,100,100);//清除矩形（清除矩形范围内像素）
//可以设置定时器做擦除的动画
function myClear(x,y,height,width,time,ctx){
}

//分步完成填充/描边矩形

ctx.beginPath();//开始路径
ctx.moveTo(200,200);//移动画笔到(200,200)
ctx.lineTo(300,200);//画笔连线到(300,200)
ctx.lineTo(300,300);//画笔连线到(300,300)
ctx.lineTo(200,300);//画笔连线到(200,300)
//或者用rect方法直接选出矩形
ctx.closePath();//闭合路径
ctx.stroke();//描边
ctx.fill();//填充
```

绘制圆
