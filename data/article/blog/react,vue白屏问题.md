# React、Vue打包后白屏问题

问题

React、Vue打包后，直接访问本地文件，或者网络服务器上没有放在根路由，会导致白屏。控制台输出是js文件404

解决

package里将homepage设置为"."