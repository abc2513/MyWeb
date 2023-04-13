# less学习笔记

css预处理语言，可运行在浏览器/node



安装

```
npm i -g less
```

使用

命令行编译成css

```
lessc styles.less styles.css
```

浏览器端

js放在less后面，因为不是依赖js，而是用js编译

```html
<style type="text/less"></style>
<script src="less/less"></script>
```



