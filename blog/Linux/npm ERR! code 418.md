今天在测试环境下拉代码的时候，发现拉不下来，修改了代理proxy并且清除了npm的缓存，发现并没有什么卵用，出现了code418错误，是因为npm的**registry**改成了**https**安全认证

清除npm缓存
```
npm cache clean --force 
```

**错误报告：**

```
npm ERR! code 418
npm ERR! I'm a teapot:
```


**解决方法：**

```
npm config set registry=https://registry.npmjs.org/
```

