# Github Timeout 问题

问题：

github在境外，有时候访问不上，使用clash代理后可以访问，但是代码还是推送不上去

解决：

git需要手动设置代理，设置成本地的clash，默认端口号是7890

```bash
#设置代理
git config --global http.proxy http://127.0.0.1:7890 
git config --global https.proxy http://127.0.0.1:7890
```

这样git就能使用clash的代理服务进行推送代码啦

```bash
# 取消代理
git config --global --unset http.proxy
git config --global --unset https.proxy

# 查看代理
git config --global --get http.proxy
git config --global --get https.proxy
```

