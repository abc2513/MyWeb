#### 脚手架配置代理

##### 单个配置

在package.json加

```js
"proxy":"http://xxx.xxx"
```

##### 多个配置

src/setupProxy.js

```js
const proxy=require('http-proxy-middleware')
module.exports=function(app){
    app.use(
        proxy('/api',{
            	target:"http://movie.douban.com",
               	changeOrigin:true,
               	pathRewrite:{'^/api':''},
        })
    )
}
```



#### 消息订阅与发布

##### 加载

```js
import PubSub from 'pubsub-js'
```

##### 订阅

```js
var token=PubSub.subscribe('消息名',(msg,data)=>{});
```

##### 取消订阅

```js
PubSub.unsubscribe(token);//取消一个订阅
PubSub.unsubscribe(回调)；//取消一个函数对应的所有订阅
```

##### 发布

```js
PubSub.publish('消息名',data);
```



#### fetch发请求

fetch是window内置的，和xhr同级，不使用xhr发送请求

fetchAPI是Promise风格设计，解决了xhr不符合关注分离的原则

兼容性不高，老版本浏览器不行

```js
fetch(url,{})
    .then(
        (res)=>{return response.json()},//链接服务器成功（非200的状态码也一样）
        (err)=>{return new Promise()}
    )
	.then(
    	(res)=>{},//获取数据成功（200）
    	(err)=>{}
	)
```

```js
//统一处理错误
fetch(url,{})
    .then(
        (res)=>{return response.json()},//链接服务器成功（非200的状态码也一样）
    )
	.then(
    	(res)=>{},//获取数据成功（200）
	)
	.catch(err=>{})
```



```js
async()=>{
    try{
        const response=await fetch(url)
        const data=await response.json()//promise实例
    }catch(e){

    }
}
```



#### 路由Router5

```js
react-router-dom@5
```

##### 基本使用

- 根放HashRouter/
- 导航区写Link
- 展示区写Route

##### 路由组件与一般组件

写法

- 路由组件`<Route path component/>`
- 一般组件`<Demo></Demo>`

props

- 路由组件默认接受三个属性
  - history对象
  - location对象
  - match对象
- 一般组件：自行指定

![image-20230329161711554](react笔记.assets/image-20230329161711554.png)

存放

- 路由组件：pages
- 一般组件：components

##### NavLink和封装

点击链接后应用样式

```css
<NavLink activeClassName="xxx"></NavLink>
```

封装NavLink成组件，避免重复样式：样式写死，props传入to；标签体通过this.props.children传入内容

##### Switch

注册路由的时候，在Switch内的Route匹配到一个就不会继续匹配下去了。单一匹配，提高效率

##### 解决多级路径样式丢失问题

原因：多级路径路由导致样式文件路径出错，返回index.html

解决：

1. `./`改写成`/`
2. 使用%PUBLIC_URL%获取public文件夹位置，只限router
3. 使用hashRouter

##### 模糊匹配和严格匹配

默认模糊匹配，Link尾部给多了可以匹配上

严格匹配`<Route exact />`，不要随便开，有时候可能会出现无法匹配二级路由的问题

##### Redirect

重定向

`<Redirect to="/about"/>`

写在末尾作为默认值

##### 嵌套路由

路径要写完整路径

##### 传递参数

###### params参数

路径传入：

```js
{`xxx/xxx/abc=${a}`}
```

在注册路由route处声明接收参数

```jsx
<Route path="xxx/:id" ../>
```

访问数据

```js
props.match.params.id
```

###### search参数

路径传入：

```js
xxx.xxx?abc=a
```

路由无需声明接收

```
props.history.location.search
```

数据是？xxx=xx的形式，需要自行转化

引入querystring库转化

```js
import qs from 'querystring'
qs.stringify(obj)
qs.parse(str)
```

###### state参数

传入对象

```js
to={{pathname:'xxx.xxx',state:{}}}
```

无需声明接收

访问数据

```
props.history.location.state
```

##### push和replace

link里加replace

##### 编程式路由导航

this.props.history.push('xxx/xxx')

this.props.history.replace('xxx/xxx')

携带state参数

this.props.history.push('xxx/xxx',{})

this.props.history.replace('xxx/xxx',{})

this.props.history.push('xxx/xxx')

this.props.history.go(-1)

goBack()

goForward()

##### withRouter

在非路由组件里操作路由

```js
import {withRouter} from 'react-router-dom'
一般组件............
export default withRouter(一般组件)
```

把一般组件加工成路由组件,让一般组件具有路由组件的API

BrowserRouter和HashRouter

![image-20230330111730394](react笔记.assets/image-20230330111730394.png)



#### UI组件库



#### redux

用于集中式状态管理的JS库（不是react插件库）

不用时困难才用



工作流程

![image-20230330123903075](react笔记.assets/image-20230330123903075.png)



































#### 项目打包

```bash
npm build
```



#### 路由Router6

![image-20230330124931355](react笔记.assets/image-20230330124931355.png)



##### 安装

```bash
npm i ...
```



##### 变化

- `component={About}``element={<About/>}`
- `<Switch>`改成`<Routes>`且必须写



##### 重定向

渲染`<Navigate/>`即可引起路由变化

`<Route path="/" element={}>`

`{sum==2?<Navigate to="/about"/>:'000000'}`



##### 子路由





















































