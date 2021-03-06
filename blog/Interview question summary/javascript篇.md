## 一、同步任务和异步任务

（1）为什么会有同步和异步

因为JavaScript是单线程，因此同个时间只能处理一个任务，所有任务都需要排队，前一个任务执行完才能执行下一个任务，但是如果前一个任务执行的时间很长，比如文件的读取操作或ajax操作，后一个任务就不得不等着，拿ajax来说，当用户向后台获取大量数据时，不得不等到所有数据都获取完毕才能进行下一步操作，用户只能在那里干等着，严重影响用户体验。

因此JavaScript在设计的时候就已经考虑到了这个问题，主线程完全不用等待文件的读取完毕或ajax的加载成功，可以先挂起处于等待中的任务，先运行排在后面的任务，等文件的读取或ajax有了响应后，再回过头执行挂起的任务，因此任务就可以分为`同步任务`和`异步任务`。

（2）同步任务

同步任务是指在`主线程上排队执行的任务，只有前一个任务执行完毕，才能继续执行下一个任务`，当我们打开网站时，网站的渲染过程，比如元素的渲染，其实就是一个同步任务。

（3）异步任务

异步任务是指`不进入主线程，而进入任务队列的任务，只有任务队列通知主线程，某个异步任务可以执行了，该任务才会进入主线程`，当我们打开网站时，图片的加载，音乐的加载，其实就是一个异步任务。

（4）异步机制

JavaScript的异步机制包括以下几个步骤：

 1. 所有同步任务都在主线程生执行，形成一个执行栈；
 2. 主线程之外还存在一个任务队列，只要异步任务有了结果，就会在任务队列中放置一个事件；
 3. 一旦执行栈中的所有同步任务执行完毕，系统就会读取异步任务，于是结束等待状态，进入执行栈，开始执行；
 4. 主线程不断的重复上面的第三步。

## 二、异步编程

（1）回调函数

回调函数是实现异步编程最简单的方法。回调函数的优点是简单

回调函数是实现异步编程最简单的方法啦，回调函数我们在使用ajax时应该用的很多啦，其实在使用ajax时，我们就用到了异步

```	
var req = new XMLHttpRequest();
req.open("GET",url);
req.send(null);
req.onreadystatechange=function(){}
```

req.send（）方法是 AJAX  向服务器发生数据，它是一个异步任务，而 req.onreadystatechange（）属于事件回调，借由浏览器的HTTP请求线程发起对服务器的请求，在请求得到响应之后触发请求完成事件，将回调函数推入事件队列等待执行

其实像setTimeout，还有我们平时为元素绑定监听事件，和上面说的道理也是一样的

回调函数的优点是简单、容易理解和部署，缺点是不利于代码的阅读和维护，各个部分之间高度耦合（Coupling），流程会很混乱，而且每个任务只能指定一个回调函数

（2）Promise

一直以来，JavaScript处理异步都是以callback的方式，在前端开发领域callback机制几乎深入人心，近几年随着JavaScript开发模式的逐渐成熟，CommonJS规范顺势而生，其中就包括提出了Promise规范，Promise完全改变了js异步编程的写法，让异步编程变得十分的易于理解，同时Promise也已经纳入了ES6，而且高版本的chrome、firefox浏览器都已经原生实现了Promise，只不过和现如今流行的类Promise类库相比少些API

Promise包括以下几个规范

一个promise可能有三种状态：等待（pending）、已完成（fulfilled）、已拒绝（rejected）
一个promise的状态只可能从“等待”转到“完成”态或者“拒绝”态，不能逆向转换，同时“完成”态和“拒绝”态不能相互转换
promise必须实现then方法（可以说，then就是promise的核心），而且then必须返回一个promise，同一个promise的then可以调用多次，并且回调的执行顺序跟它们被定义时的顺序一致
then方法接受两个参数，第一个参数是成功时的回调，在promise由“等待”态转换到“完成”态时调用，另一个是失败时的回调，在promise由“等待”态转换到“拒绝”态时调用，同时，then可以接受另一个promise传入，也接受一个“类then”的对象或方法，即thenable对象

在使用Promise时，我们需要检测一些浏览器是否支持Promise