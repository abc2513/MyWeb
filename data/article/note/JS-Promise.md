# Promoise

## 一、准备

## 二、理解

### 简介

#### 介绍

- 是ES6一门新的技术，JS中进行异步变成的新解决方案（旧方案是使用回调）
- 是一个构造函数，用来封装一个异步操作并获取结果值

异步操作：fs、数据库操作、AJAX、定时器

原来用回调函数进行异步操作

#### 优点

1. 指定回调函数的方式灵活
2. 支持链式调用。解决了回调地狱的问题，便于阅读和异常处理

### 基本使用

```js
const p=new Promise((resolve,reject)=>{//传入一个函数，参数里有resolve、reject
    //异步任务
    setTimeout(()=>{
        if(xxx)
            resolve();//调用参数里的resolve，将对象状态设为成功
        	//resolve(value)传递数据	
        else
            reject();//调用参数里的reject将对象状态设为失败
        	//reject(reason)传递数据	
    },1000)
})
p.then(
    ()=>{},//第一个参数是成功的处理(value)=>{}
    ()=>{}//第二个参数是失败的处理(reason)=>{}
)
```

#### fs读取文件

```js
//直接使用
const fs=require('fs')
fs.readFile('xxx',(err,data)=>{
    if(err) throw err;
    console.log(data.toString())
})
//Promise
let p=new Promise((resolve,reject)=>{
    fs.readFile('xxx',(err,data)=>{
        if(err) reject(err)
        else resolve(data)
    })
})
p.then(
    (value)=>{},
    (reason)=>{}
)
```

#### AJAX

### 封装练习

```js
//封装
function mineReadFile(path){
    return new Promise((resolve,reject)=>{
        const xhr=new XMLHttpRequest();
        xhr.open(url);
        xhr.onStageReadyChange=()=>{
            
        }
        xhr.send();
    });
}
//调用
mineReadFile('xxx').then(
	()=>{},
    ()=>{}
)
```

### `util.promisify()`

直接将错误优先、回调函数风格的方法进行Promise风格化

```js
//封装
const util=require('util');
let mineReadFile=util.promisify(fs.readFile)
//调用
mineReadFile('xxx').then(
	()=>{},
    ()=>{}
)
```

### 属性

#### 状态属性

- `实例对象.PromiseState`，内置，不能直接操作
- pending=>resolved/rejected，只能改变一次
- 未决定的、成功、失败

#### 结果值属性

保存着成功/失败的结果

- `实例对象.PromiseResult`

### 工作流程

![image-20230328200211517](Promise笔记.assets/image-20230328200211517.png)

### API

#### 构造函数

```js
new Promise((resolve,reject)=>{
    //executor函数,这里的代码在Promise内部立即同步调用，不会进入队列
})
```

#### 实例对象

##### then()

`Promise.prototype.then`，传入两个处理函数参数，指定成功和失败的回调

```js
p.then(onResolved,onRejected)
```

##### catch()

`Promise.prototype.catch`，指定失败的回调

```js
p.catch(onRejected)
```

#### 函数对象

##### resolve()

快速返回一个promise成功结果

- 如果传入非promise类型的对象，返回结果为成功的promise对象
- 如果传入promise类型的对象，参数promise的结果决定了返回的结果

```js
let promise1=Promise.resolve()
```

##### reject()

快速返回一个promise失败结果，无论传入什么参数

##### all()

传入promise数组，全部成功时返回成功

##### race()

传入promise数组，第一个改变状态的决定结果

### 几个关键问题

#### 1. 如何修改对象状态

resolve, reject, catch

#### 2. 能否执行多个成功/失败回调

能

```js
p.then(()=>{1})
p.then(()=>{2})
```

#### 3. 改变状态与指定回调先后顺序

先改状态的情况

- 在执行器中直接调用resolve()/reject()
- 延迟后才调用then()

先指定回调的情况

- 在执行器异步操作后调用resolve()/reject() （正常情况）

##### 什么时候拿到数据

- 先指定回调，则状态改变时
- 先改变状态，则指定回调时

#### 4. then返回由什么决定

由then指定的回调函数执行的结果决定

![image-20230328210853106](Promise笔记.assets/image-20230328210853106.png)

#### 5. 串联多个任务

```js
p.then(value=>{
    return new Promise((resolve,reject)=>{...})
}).then(value=>{})//新的处理
```

#### 6. 异常穿透

```js
p.then(value=>{
    throw '失败！'
    return new Promise((resolve,reject)=>{...})
}).then(value=>{
    return new Promise((resolve,reject)=>{...})
}).then(value=>{
    return new Promise((resolve,reject)=>{...})
}).then(value=>{
    return new Promise((resolve,reject)=>{...})
}).catch(reason=>{...})
```



#### 7. 中断promise链

throw/return null无效

有且仅有一个方法：返回一个panding的promise

```js
p.then(value=>{
    return new Promise(()=>{
        //既没有执行resolve也没有reject
    })
}).then(value=>{
}).then(value=>{
}).then(value=>{
}).catch(reason=>{...})
```

状态是panding，后续的回调一直不能执行

## 三、自定义封装Promise

初始结

## 四、async和await标识符

### async 函数

async标识的函数，返回结果是promise对象，promise的结果由async函数执行的返回值决定

- 返回值非promise：promise的结果是成功
- 返回值为promise：promise的结果由其决定
- throw：失败

```js
async function test(){
    
}
let result=test();
```

### await 表达式

- await右侧的表达式是promise对象，返回promise成功的值
- await右侧的表达式不是promise对象，直接返回该值



- await必须写在async函数内
- await后的promise结果失败时，会抛出异常，需要try...catch...捕获

### 综合实践























































