## mockjs的作用

- 生成模拟数据
- 模拟 Ajax 请求，返回模拟数据
- 基于 HTML 模板生成模拟数据（后续更新）
- 帮助编写单元测试（后续更新）

## Vue 中使用 mock
有两种使用方式，一种是仅编写数据来调用，第二种是编写 服务+数据模拟真实接口（可在network查看）
### 第一种方式 
使用mock的方式模拟数据接口
- 可以写完整action url：http://localhost/login
- 同一域下，也可以只写api形式：/login
- 使用 Mock.mock 可以编写多个接口数据
```javascript
import Mock from 'mockjs'; //es6语法引入mock模块

Mock.mock('/login', { //输出数据
    'name': '@name', //随机生成姓名
    //还可以自定义其他数据
});
Mock.mock('/list', { //输出数据
    'age|10-20': 10
    //还可以自定义其他数据
});
```
调用的时候直接调用即可
```javascript
import "../mock/mock.js";
axios.post("/login").then(response => {
    if (response.data) {
      this.jumpToKpi();
    }
});
```
### 第二种方式，编写server
**使用express提供来服务**
- 首先需要安装依赖：express、hotnode（hotnode提供热加载）


```javascript
## 安装 express 和 hotode
npm install express hotnode --save-dev
## 启动服务命令
hotnode mockService.js
```
**跨域设置**

假设 express 服务使用 8090 端口，而 vue 开发一般启动 8080，则必定跨域，需在 server 端进行设置

```javascript
app.use(function (req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header('Access-Control-Allow-Methods', 'PUT, GET, POST, DELETE, OPTIONS');
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    res.header('Access-Control-Allow-Headers', 'Content-Type');
    next();
});
```

**在chrome network下可查看到接口调用**

```javascript
//请求接口地址
http://localhost:3001/list
```
**完整的 mockservice.js 如下**
```javascript
// import express from "express"
// import Mock from "mockjs"
const express = require('express');   //引入express
const Mock = require('mockjs');       //引入mock

const port = '3001'
let app = express();
const Random = Mock.Random;

app.use(function (req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header('Access-Control-Allow-Methods', 'PUT, GET, POST, DELETE, OPTIONS');
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    res.header('Access-Control-Allow-Headers', 'Content-Type');
    next();
});

app.use('/list', function (req, res) {
    res.json(Mock.mock({
        'list|1-10': [{
            'id|+1': 1
        }]
    }))
});

app.listen(port, () => {
    console.log('监听端口 ' + port)
})
```
```javascript
// ==>
{
    "list": [
        {
            "id": 1
        },
        {
            "id": 2
        },
        {
            "id": 3
        }
    ]
}
```

## 关于mockjs的语法规范

Mock.js 的语法规范包括两部分：

- 1.数据模板定义规范（Data Template Definition，DTD）
- 2.数据占位符定义规范（Data Placeholder Definition，DPD）

### 1.数据模板定义规范 DTD
**数据模板中的每个属性由 3 部分构成：属性名、生成规则、属性值：**

```javascipt
// 属性名   name
// 生成规则 rule
// 属性值   value
'name|rule': value
```
更多语法规范请查找官方链接 https://github.com/nuysoft/Mock/wiki/Syntax-Specification

这里只介绍部分生成规则和示例：
1.属性值是字符串 String
2.属性值是数字 Number
  - 'name|+1': number

    属性值自动加 1，初始值为 number。

  - 'name|min-max': number
    
    生成一个大于等于 min、小于等于 max 的整数，属性值 number 只是用来确定类型。

  - 'name|min-max.dmin-dmax': number
    
    生成一个浮点数，整数部分大于等于 min、小于等于 max，小数部分保留 dmin 到 dmax 位。
```javascript
Mock.mock({
    'number1|1-100.1-10': 1,
    'number2|123.1-10': 1,
    'number3|123.3': 1,
    'number4|123.10': 1.123
})
// =>
{
    "number1": 12.92,
    "number2": 123.51,
    "number3": 123.777,
    "number4": 123.1231091814
}
```

### 2.数据占位符定义规范 DPD

占位符 只是在属性值字符串中占个位置，并不出现在最终的属性值中。

占位符 的格式为：

```javascript
@占位符
@占位符(参数 [, 参数])
```

**注意：**

1.用 @ 来标识其后的字符串是 占位符。

2.占位符 引用的是 Mock.Random 中的方法。

3.通过 Mock.Random.extend() 来扩展自定义占位符。

4.占位符 也可以引用 数据模板 中的属性。

5.占位符 会优先引用 数据模板 中的属性。

6.占位符 支持 相对路径 和 绝对路径。

```javascript
Mock.mock({
    name: {
        first: '@FIRST',
        middle: '@FIRST',
        last: '@LAST',
        full: '@first @middle @last'
    }
})
// =>
{
    "name": {
        "first": "Charles",
        "middle": "Brenda",
        "last": "Lopez",
        "full": "Charles Brenda Lopez"
    }
}
```

## Mock.mock()

### Mock.mock( rurl?, rtype?, template|function( options ) )
根据数据模板生成模拟数据。

示例如下

**Mock.mock( rurl, rtype, template )**

记录数据模板。当拦截到匹配 rurl 和 rtype 的 Ajax 请求时，将根据数据模板 template 生成模拟数据，并作为响应数据返回。

```javascript
Mock.mock(/\.json/, 'get', {
    type: 'get'
})
Mock.mock(/\.json/, 'post', {
    type: 'post'
})

$.ajax({
    url: 'hello.json',
    type: 'get',
    dataType: 'json'
}).done(function (data, status, jqXHR) {
    $('<pre>').text(JSON.stringify(data, null, 4))
        .appendTo('body')
})

$.ajax({
    url: 'hello.json',
    type: 'post',
    dataType: 'json'
}).done(function (data, status, jqXHR) {
    $('<pre>').text(JSON.stringify(data, null, 4))
        .appendTo('body')
})
```

## Mock.setup()

**Mock.setup( settings )**

配置拦截 Ajax 请求时的行为。支持的配置项有：timeout。

指定被拦截的 Ajax 请求的响应时间，单位是毫秒。值可以是正整数，例如 400，表示 400 毫秒 后才会返回响应内容；也可以是横杠 '-' 风格的字符串，例如 '200-600'，表示响应时间介于 200 和 600 毫秒之间。默认值是'10-100'。

```javascript
Mock.setup({
    timeout: 400
})
Mock.setup({
    timeout: '200-600'
})
```
>目前，接口 Mock.setup( settings ) 仅用于配置 Ajax 请求，将来可能用于配置 Mock 的其他行为。

## Mock.Random

Mock.Random 是一个工具类，用于生成各种随机数据。

Mock.Random 的方法在数据模板中称为『占位符』，书写格式为 @占位符(参数 [, 参数]) 。

```javascript
var Random = Mock.Random
Random.email()
// => "n.clark@miller.io"
Mock.mock('@email')
// => "y.lee@lewis.org"
Mock.mock( { email: '@email' } )
// => { email: "v.lewis@hall.gov" }
```

**注意**

### 方法
Mock.Random 提供的完整方法（占位符）如下：

|Type |	Method |
|--|--|
|Basic |	boolean, natural, integer, float, character, string, range, date, time, datetime, now |
|Image |	image, dataImage |
|Color |	color |
|Text |	paragraph, sentence, word, title, cparagraph, csentence, cword, ctitle |
|Name |	first, last, name, cfirst, clast, cname |
|Web |	url, domain, email, ip, tld |
|Address |	area, region |
|Helper |	capitalize, upper, lower, pick, shuffle |
|Miscellaneous |	guid, id |

### 扩展
Mock.Random 中的方法与数据模板的 @占位符 一一对应，在需要时还可以为 Mock.Random 扩展方法，然后在数据模板中通过 @扩展方法 引用。例如：

```javascript
Random.extend({
    constellation: function(date) {
        var constellations = ['白羊座', '金牛座', '双子座', '巨蟹座', '狮子座', '处女座', '天秤座', '天蝎座', '射手座', '摩羯座', '水瓶座', '双鱼座']
        return this.pick(constellations)
    }
})
Random.constellation()
// => "水瓶座"
Mock.mock('@CONSTELLATION')
// => "天蝎座"
Mock.mock({
    constellation: '@CONSTELLATION'
})
// => { constellation: "射手座" }
```

## Mock.valid()

### Mock.valid( template, data )
- Mock.valid( template, data )

校验真实数据 data 是否与数据模板 template 匹配。

**template**

表示数据模板，可以是对象或字符串。例如 { 'list|1-10':[{}] }、'@EMAIL'。

**data**

表示真实数据。

```javascript
var template = {
    name: 'value1'
}
var data = {
    name: 'value2'
}
Mock.valid(template, data)
// =>
[
    {
        "path": [
            "data",
            "name"
        ],
        "type": "value",
        "actual": "value2",
        "expected": "value1",
        "action": "equal to",
        "message": "[VALUE] Expect ROOT.name'value is equal to value1, but is value2"
    }
]
```