# NodeJS

基本服务器

步骤

1. 导入http模块
2. 创建服务器实例
3. 监听请求
4. 启动服务器

安装包

```js
http
```

代码

```js
const http = require('http')
const server=http.createServer()
const express
server.on('request',(req,res)=>{
    console.log('Someone visiting')
})

server.listen(3333,()=>{
    console.log('start!')
})
```

express

基本服务器

```js
const express=require('express')
const app=express()
app.use(express.static('./data'))
app.listen(6666,()=>{
    console.log('start!')
})
```

