## 什么是闭包？

>当一个函数能够记住并访问到其所在的词法作用域及作用域链，特别强调是在其定义的作用域外进行的访问，此时该函数和其上层执行上下文共同构成闭包。

**需要明确的几点：**

- 1.闭包一定是函数对象
- 2.闭包和词法作用域、作用域链、垃圾回收机制息息相关
- 3.当函数一定是在其定义的作用域外进行访问时，才产生闭包
- 4.闭包是由该函数和其上层执行上下文共同构成


## 闭包的应用需要注意的事项：
>闭包在js中让很多不可能实现的代码成为可能，虽然好用，但是也要合理使用。

>闭包阻止了垃圾回收机制对其的回收，因此变量会一直存在内存中，即使当变量不在使用页依然存在，这样会造成内存泄露，会严重影响页面的性能，因此方变量不在使用时，我们要将其释放。
就以上面的代码为例：

```
function foo() {
    let a = 2;

    function bar() {
        console.log(a);
    }

    return bar;
}

let baz = foo();

baz(); //baz指向的对象会永远存在于堆内存中

baz = null; //如果不再使用，将其指向的对象释放
```

## 闭包的应用

### 1.模块化 
一个模块具有私有属性、私有方法和公有属性、公有方法。闭包能将模块的公有属性、方法暴漏出来。
```
var myModule = (function () {
    let name = "lenhart";

    function getName() {
        return name;
    }

    return {
        name,
        getName
    }
})();

console.log(myModule.name); //lenhart

console.log(myModule.getName()); //lenhart
```

### 2.定时器
延时器（setTimeout），计数器（setInterval），这里简单些一个常见的闭包面试题。

```
for (var i=0; i<5; i++) {
    setTimeout(()=>{
        console.log(i);
    }, i*1000)
}
// 每秒钟输出一个5，一共输出5次
```
那么如何做到每秒钟输出一个数，依次为0,1,2,3,4
```
for (var i = 0; i < 5; i++) {
    ((j) => {
        setTimeout(() => {
            console.log(j);
        }, 1000 * j)
    })(i)
}
```

### 3.监听器
```
var oDiv = documenet.querySelector("#div");
oDiv.addEventListener('click', ()=>{
    console.log(oDiv.id);
});
```
