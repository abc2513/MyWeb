# PixiJS笔记

图形库，高效、兼容性好、api简洁

https://pixijs.com/

```bash
npm i pixi.js
```

### 基本使用

```js
import { useEffect } from 'react';
import './App.css';
import * as PIXI from 'pixi.js';
const app = new PIXI.Application({
  width: window.innerWidth,
  height: window.innerHeight,
  backgroundColor: 0x1099bb,
  resolution: window.devicePixelRatio || 1,//设备的像素比
});
function App() {
  useEffect(() => {
    document.body.appendChild(app.view);


    //创建矩形
    const rectangle = new PIXI.Graphics();
    rectangle.beginFill(0x66CCFF,1);//填充颜色,透明度
    rectangle.drawRect(0, 0, 64, 64);//绘制矩形，参数：x,y,width,height
    rectangle.endFill();
    //位置
    rectangle.x = 170;
    rectangle.y = 170;
    //缩放
    rectangle.scale.x = 2;
    rectangle.scale.y = 2;
    rectangle.scale.set(2, 2);//等同于上面，函数写法
    //位移
    rectangle.position.x = 170;
    //旋转
    rectangle.rotation = 0.5;
    //锚点：旋转和缩放的中心点
    rectangle.pivot.set(32, 32);
    //边框
    rectangle.lineStyle(4, 0xFF3300, 1);
    rectangle.drawRect(0, 0, 64, 64);
    //添加进舞台
    app.stage.addChild(rectangle);
  }, [])
  return (
    <div className="App">
    </div>
  );
}

export default App;

```

### 几种基本图形

```js
//创建圆角矩形
const roundBox = new PIXI.Graphics();
roundBox.lineStyle(4, 0x99CCFF, 1);
roundBox.beginFill(0xFF9933);
roundBox.drawRoundedRect(0, 0, 84, 36, 10)//绘制圆角矩形，参数：x,y,width,height,radius
roundBox.endFill();
roundBox.position.set(248, 190);
app.stage.addChild(roundBox);
//绘制椭圆
const ellipse = new PIXI.Graphics();
ellipse.beginFill(0xFFFF00);
ellipse.drawEllipse(0, 0, 50, 20);//绘制椭圆，参数：x,y,width,height
ellipse.endFill();
ellipse.position.set(48, 190);
app.stage.addChild(ellipse);

//绘制多边形
const path = new PIXI.Graphics();
path.beginFill(0x33FF33);
path.drawPolygon([50, 0, 100, 100, 0, 100]);//绘制多边形，参数：点的数组
path.endFill();
path.position.set(48, 190);
app.stage.addChild(path);

//绘制圆弧
const arc = new PIXI.Graphics();
arc.lineStyle(4, 0x99CCFF, 1);
arc.beginFill(0xFF9933);
arc.arc(300, 0, 50, 0, Math.PI / 2);//绘制圆弧，参数：x,y,radius,startAngle,endAngle
arc.endFill();
arc.position.set(48, 190);
app.stage.addChild(arc);

//绘制线段
const line = new PIXI.Graphics();
line.lineStyle(4, 0xFFFFFF, 1);
line.moveTo(0, 0);//起点
line.lineTo(80, 50);//终点
line.position.set(48, 190);
app.stage.addChild(line);

//绘制折线
const line2 = new PIXI.Graphics();
line2.lineStyle(4, 0xFFFFFF, 1);
line2.moveTo(0, 0);//起点
line2.lineTo(80, 50);//终点
line2.lineTo(80, 100);//终点
line2.lineTo(0, 100);//终点
line2.lineTo(0, 0);//终点
line2.position.set(48, 190);
app.stage.addChild(line2);

//绘制贝塞尔曲线
const bezier = new PIXI.Graphics();
bezier.lineStyle(4, 0xFFFFFF, 1);
bezier.moveTo(0, 0);//起点
bezier.bezierCurveTo(0, 50, 50, 50, 50, 100);//终点
bezier.position.set(48, 190);
app.stage.addChild(bezier);

```

### 添加纹理

```js
    //创建纹理
    const texture = PIXI.Texture.from('./favicon.ico');
    //创建精灵(图形对象，在上面添加纹理)
    const sprite = new PIXI.Sprite(texture);
    //完全居中
    sprite.anchor.set(0.5);
    sprite.position.set(app.screen.width / 2, app.screen.height / 2);
    app.stage.addChild(sprite);
```

### 简单动画

```js
    //动画
    app.ticker.add((delta) => {
      sprite.rotation += 0.01 * delta;
    }
    );
    //添加点击事件，控制动画开关
    sprite.interactive = true;
    sprite.on('pointerdown', () => {
      app.ticker.stop();
    }
    );
    sprite.on('pointerup', () => {
      app.ticker.start();
    } 
    );
```

### 资源管理

```js

```

#### 场景

```js
    //添加场景1的资源
    PIXI.Assets.addBundle('scene1', {
      a:'./a.png',
      b:'./b.png',
      c:'./c.png',
    });
    //加载资源
    PIXI.Assets.loadBundle('scene1',(process)=>{
      console.log('加载进度',process);
    }).then((textures) => {
      //创建精灵
      const spriteA = new PIXI.Sprite(textures.a);
      const spriteB = new PIXI.Sprite(textures.b);
      const spriteC = new PIXI.Sprite(textures.c);
      //设置精灵的位置
      spriteA.x = 100;
      spriteA.y = 100;
      spriteB.x = 200;
      spriteB.y = 100;
      spriteC.x = 300;
      spriteC.y = 100;
      //添加到舞台
      app.stage.addChild(spriteA);
      app.stage.addChild(spriteB);
      app.stage.addChild(spriteC);
    }
    );
```

### 文字

```js
    //添加文字
    const basicText = new PIXI.Text('Basic text in pixi', {
      //样式
      fill: 0xffffff,
      fontSize: 36,
      fontFamily: 'Arial',
    });
    //居中
    basicText.anchor.set(0.5);
    //设置位置
    basicText.x = app.screen.width / 2;
    basicText.y = app.screen.height / 2;
    
    app.stage.addChild(basicText);

```

### 遮罩

```js
//添加文字
const basicText = new PIXI.Text('Basic text in pixi', {
    //样式
    fill: 0xffffff,//要有填充才能起到作用
    fontSize: '120px',
    fontFamily: 'Arial',
});
basicText.anchor.set(0.5);
basicText.x = app.screen.width / 2;
basicText.y = app.screen.height / 2;

//添加背景
const bg = PIXI.Sprite.from('./bg.jpg');
bg.anchor.set(0.5);
bg.x = app.screen.width / 2;
bg.y = app.screen.height / 2;

//设置遮罩
bg.mask = basicText;
app.stage.addChild(bg);
```

### 滤镜

模糊滤镜

```js
    //创建一个纹理
    const texture = PIXI.Texture.from('./logo512.png');
    const bunny = new PIXI.Sprite(texture);
    bunny.x = app.renderer.width / 2;
    bunny.y = app.renderer.height / 2;
    bunny.anchor.x = 0.5;
    bunny.anchor.y = 0.5;
    app.stage.addChild(bunny);

    //创建模糊滤镜
    const blurFilter = new PIXI.BlurFilter();
    blurFilter.blur = 10;
    //将模糊滤镜添加到精灵上
    bunny.filters = [blurFilter];
    //鼠标进入精灵时，模糊度变为0
    bunny.interactive = true;
    bunny.on('pointerover', () => {
      blurFilter.blur = 0;
    });
    //鼠标离开精灵时，模糊度变为10
    bunny.on('pointerout', () => {
      blurFilter.blur = 10;
    });
```

#### 使用滤镜库

http://github.com/pixijs/filters

```bash
npm i pixi-filters
```



轮廓滤镜、发光滤镜

```js
    //创建一个纹理
    const texture = PIXI.Texture.from('./logo512.png');
    const bunny = new PIXI.Sprite(texture);
    bunny.x = app.renderer.width / 2;
    bunny.y = app.renderer.height / 2;
    bunny.anchor.x = 0.5;
    bunny.anchor.y = 0.5;
    app.stage.addChild(bunny);

   
    //轮廓滤镜
    const outlineFilterBlue = new OutlineFilter(2, 0x99ff99);
    //发光滤镜
    const glowFilter = new GlowFilter(15, 2, 1, 0xff0000, 0.5);
    bunny.filters = [outlineFilterBlue, glowFilter];
```

波纹滤镜

```js
    //波纹滤镜
    const filter = new ShockwaveFilter([
      app.screen.width *Math.random(),
      app.screen.height*Math.random()
    ],{
      radius: 2000,//波纹的半径
      amplitude: 10,//波纹的振幅
      wavelength: 80,//波纹的波长
      brightness: 1,//波纹的亮度
      speed: 200,//波纹的速度
    });
    filter.center = [app.screen.width / 2, app.screen.height / 2];
    filter.time = 0;
    app.stage.filters = [filter];
    function createWave(waveFilter,resTime){
      waveFilter.time += 0.01;
      if (waveFilter.time > resTime) {
        waveFilter.time = 0;
        waveFilter.center = [
          app.screen.width * Math.random(),
          app.screen.height * Math.random(),
        ];
      }
    }
    app.ticker.add(() => {
      createWave(filter,100);
    }
    );
    //鼠标点击
    app.stage.interactive = true;
    app.stage.on('pointerdown', (event) => {
      filter.center = [event.data.global.x, event.data.global.y];
      filter.time=0;
    }
    );
```

