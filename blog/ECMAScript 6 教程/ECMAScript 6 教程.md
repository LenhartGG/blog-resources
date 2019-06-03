ECMAScript 6 目前基本成为业界标准，它的普及速度比 ES5 要快很多，主要原因是现代浏览器对 ES6 的支持相当迅速，尤其是 Chrome 和 Firefox 浏览器，已经支持 ES6 中绝大多数的特性。
![这里写图片描述](https://github.com/LenhartGG/blog-resources/blob/master/img/wechart/ES6.png?raw=true)
### 1. let、const 和 block 作用域
let 允许创建块级作用域，ES6 推荐在函数中使用 let 定义变量，而非 var：

```
var a = 2;
{
  let a = 3;
  console.log(a); // 3
}
console.log(a); // 2
```
同样在块级作用域有效的另一个变量声明方式是 const，它可以声明一个常量。ES6 中，const 声明的常量类似于指针，它指向某个引用，也就是说这个「常量」并非一成不变的，如：

```
{
  const ARR = [5,6];
  ARR.push(7);
  console.log(ARR); // [5,6,7]
  ARR = 10; // TypeError
}
```
有几个点需要注意：

 - let 关键词声明的变量不具备变量提升（hoisting）特性
 - let 和 const 声明只在最靠近的一个块中（花括号内）有效
 - 当使用常量 const 声明时，请使用大写变量，如：CAPITAL_CASING
 - const 在声明时必须被赋值

### 2. 箭头函数（Arrow Functions）
ES6 中，箭头函数就是函数的一种简写形式，使用括号包裹参数，跟随一个 =>，紧接着是函数体：

```
var getPrice = function() {
  return 4.55;
};
 
// Implementation with Arrow Function
var getPrice = () => 4.55;
```
需要注意的是，上面栗子中的 getPrice 箭头函数采用了简洁函数体，它不需要 return 语句，下面这个栗子使用的是正常函数体：

```
let arr = ['apple', 'banana', 'orange'];
 
let breakfast = arr.map(fruit => {
  return fruit + 's';
});
 
console.log(breakfast); // ["apples", "bananas", "oranges"]
```
当然，箭头函数不仅仅是让代码变得简洁，函数中 this 总是绑定总是指向对象自身。具体可以看看下面几个栗子：

```
function Person() {
	this.age = 0;
	
	setInterval(function growUp() {
		// 在非严格模式下，growUp() 函数的 this 指向 window 对象
		this.age++;
	}, 1000);
}
var person = new Person();
```
我们经常需要使用一个变量来保存 this，然后在 growUp 函数中引用：

```
function Person() {
  var self = this;
  self.age = 0;
 
  setInterval(function growUp() {
    self.age++;
  }, 1000);
}

```
而使用箭头函数可以省却这个麻烦：

```
function Person(){
  this.age = 0;
 
  setInterval(() => {
    // |this| 指向 person 对象
    this.age++;
  }, 1000);
}
 
var person = new Person();
```
### 3. 函数参数默认值
ES6 中允许对函数参数设置默认值：

```
let getFinalPrice = (price, tax=0.7) => price + price * tax;
getFinalPrice(500); // 850
```

### 4. Spread / Rest 操作符
Spread / Rest 操作符指的是 ...，具体是 Spread 还是 Rest 需要看上下文语境。

当被用于迭代器中时，它是一个 Spread 操作符：

```
function foo(x,y,z) {
  console.log(x,y,z);
}
 
let arr = [1,2,3];
foo(...arr); // 1 2 3
```
当被用于函数传参时，是一个 Rest 操作符：

```
function foo(...args) {
  console.log(args);
}
foo( 1, 2, 3, 4, 5); // [1, 2, 3, 4, 5]
```
### 5. 对象词法扩展
ES6 允许声明在对象字面量时使用简写语法，来初始化属性变量和函数的定义方法，并且允许在对象属性中进行计算操作：

```
function getCar(make, model, value) {
  return {
    // 简写变量
    make,  // 等同于 make: make
    model, // 等同于 model: model
    value, // 等同于 value: value
 
    // 属性可以使用表达式计算值
    ['make' + make]: true,
 
    // 忽略 `function` 关键词简写对象函数
    depreciate() {
      this.value -= 2500;
    }
  };
}
 
let car = getCar('Barret', 'Lee', 40000);
 
// output: {
//     make: 'Barret',
//     model:'Lee',
//     value: 40000,
//     makeKia: true,
//     depreciate: function()
// }
```
### 6. 二进制和八进制字面量
ES6 支持二进制和八进制的字面量，通过在数字前面添加 0o 或者0O 即可将其转换为二进制值：

```
let oValue = 0o10;
console.log(oValue); // 8
 
let bValue = 0b10; // 二进制使用 `0b` 或者 `0B`
console.log(bValue); // 2
```
### 7. 对象和数组解构
解构可以避免在对象赋值时产生中间变量：

```
function foo() {
  return [1,2,3];
}
let arr = foo(); // [1,2,3]
 
let [a, b, c] = foo();
console.log(a, b, c); // 1 2 3
 
function bar() {
  return {
    x: 4,
    y: 5,
    z: 6
  };
}
let {x: x, y: y, z: z} = bar();
console.log(x, y, z); // 4 5 6
```
### 8. 对象超类
ES6 允许在对象中使用 super 方法：

```
var parent = {
  foo() {
    console.log("Hello from the Parent");
  }
}
 
var child = {
  foo() {
    super.foo();
    console.log("Hello from the Child");
  }
}
 
Object.setPrototypeOf(child, parent);
child.foo(); // Hello from the Parent
             // Hello from the Child
```
### 9. 模板语法和分隔符
ES6 中有一种十分简洁的方法组装一堆字符串和变量。

 - ${ ... } 用来渲染一个变量
 - ` 作为分隔符

```
let user = 'Barret';
console.log(`Hi ${user}!`); // Hi Barret!
```
### 10. for...of VS for...in
for...of 用于遍历一个迭代器，如数组：

```
let nicknames = ['di', 'boo', 'punkeye'];
nicknames.size = 3;
for (let nickname of nicknames) {
  console.log(nickname);
}
// 结果: di, boo, punkeye
```
for...in 用来遍历对象中的属性：

```
let nicknames = ['di', 'boo', 'punkeye'];
nicknames.size = 3;
for (let nickname in nicknames) {
  console.log(nickname);
}
Result: 0, 1, 2, size
```
