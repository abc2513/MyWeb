# React 珠峰

### 3 脚手架的进阶应用

##### 暴露webpack配置

用于修改打包规则。执行命令`yarn eject`，执行前需要在git提交。暴露操作是单向的，一般在创建后就执行

- 这个过程会暴露config文件夹，包含webpack配置文件；
  - path：路径相关
  - webpack.config.js：webpack配置文件
- 暴露srcipts文件夹，包含打包等命令的脚本；
- 将webpack的各种模块添加到package并重新安装；
- script会基于node执行scripts文件夹下的脚本，不再使用react-script插件的命令，也没有eject命令了
- babel配置多了presets：[react-app]

![image-20231031182050631](React珠峰.assets/image-20231031182050631.png)

babel-preset-react-app

![image-20231031182529622](React珠峰.assets/image-20231031182529622.png)

默认配置有sass预编译语言，无需配置。less/stylus需要配置

##### 添加less支持

安装less和less-loader，在webpack配置文件添加less/less Module匹配规则和加载器

##### 脚本环境变量

![image-20231031185839223](React珠峰.assets/image-20231031185839223.png)

##### 修改兼容列表

<img src="React珠峰.assets/image-20231031190210021.png" alt="image-20231031190210021" style="zoom: 80%;" />

![image-20231031190518947](React珠峰.assets/image-20231031190518947.png)

代理服务器设置

![image-20231031191156067](React珠峰.assets/image-20231031191156067.png)

### 4 MVC模式和MVVM模式

React是前端框架类的库

![image-20231103142722382](React珠峰.assets/image-20231103142722382.png)

React采用MVC体系，Vue使用MVVM体系（双向驱动，数据双向流动）

<img src="React珠峰.assets/image-20231103143144486.png" alt="image-20231103143144486" style="zoom:150%;" />

Vue：

![image-20231103144900117](React珠峰.assets/image-20231103144900117.png)

![image-20231103145220564](React珠峰.assets/image-20231103145220564.png)

### 7 JSX底层渲染机制 创建虚拟DOM

![image-20231103150024068](React珠峰.assets/image-20231103150024068.png)

![image-20231103150046963](React珠峰.assets/image-20231103150046963.png)



### 9 函数组件的底层渲染机制

![image-20231103152653672](React珠峰.assets/image-20231103152653672.png)
