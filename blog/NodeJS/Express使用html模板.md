## Express使用html模板

**express默认使用jade模板，可以配置让其支持使用ejs或html模板。**

### 1.安装ejs

　　　在项目根目录安装ejs.

```
npm install ejs
```

### 2.引入ejs

```
var ejs = require('ejs');  //我是新引入的ejs插件
```

### 3.设置html引擎

```
app.engine('html', ejs.__express);
```

### 4.设置视图引擎

```
app.set('view engine', 'html');
```

**注：**在express搭建的服务器中，html引擎没有被配置，直接添加即可；视图引擎已配置，修改配置即可。

附上官方API：[http://www.expressjs.com.cn/4x/api.html#app.engine](http://www.expressjs.com.cn/4x/api.html#app.engine "官方api")